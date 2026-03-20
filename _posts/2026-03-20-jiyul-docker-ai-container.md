---
title: "How I Built Per-User AI Containers with Docker: The Jiyul Story"
date: 2026-03-20
categories:
  - Tech & AI
tags:
  - docker
  - openclaw
  - tauri
  - ai
  - saas
  - legal-tech
  - claude
---

> A legal & tax AI assistant built with Tauri 2 + Next.js + OpenClaw + Docker

---

## Why I Built This

OpenClaw is powerful. Per-user workspaces, persona definition, tool integration, memory — everything you need for a personal AI agent is already there.

But **productizing it is a different story**.

Three things were missing.

**First, enterprise-grade stability.** Running OpenClaw on my local machine is easy. But what happens when 1,000 strangers use it simultaneously? Does the container auto-recover when it crashes? Are resource limits enforced? Is user data properly isolated? You can only call something a "service" when you can answer "yes" to all of these.

**Second, domain expertise.** General-purpose AI gets legal and tax questions half right. It misquotes statutes, misses recent amendments, and answers with misplaced confidence. Jiyul solves this two ways. One is a persona injected via `SOUL.md` — a legal/tax professional's tone, judgment criteria, and awareness of its own limits. The other is **domain-specific skills mounted into each Docker container as read-only volumes**. Summaries of tax codes, key case rulings, filing deadlines — all living as files inside the container that the AI can reference. Since users can't touch the read-only layer, **the integrity of the knowledge base is guaranteed**.

**Third, a product layer.** User onboarding, authentication, data persistence, file attachments — OpenClaw doesn't have any of this. You have to build it to have a "service."

**Jiyul (知律) is the experiment of adding all three.**

OpenClaw's philosophy — *an isolated AI environment per user, persistent context, customizable persona* — stays intact. On top of that, I layered Docker-based container isolation for enterprise stability, and injected legal/tax domain expertise via `SOUL.md` and read-only skill volumes.

The real question was this: **"What does OpenClaw look like as a SaaS?"**

Jiyul is version one of that answer.

---

## Architecture Overview

```
Mobile/Desktop App (Tauri 2 + Next.js)
        │
        │ HTTPS
        ▼
Cloudflare Tunnel (api.tax-insight.kr)
        │
        ▼
Container Manager (Python, port 19200)
        │
   ┌────┴────┬────────┐
   ▼         ▼        ▼
Docker      Docker   ...
Container   Container
(User A)    (User B)
OpenClaw    OpenClaw
Gateway     Gateway
```

The core principle: **one user = one Docker container**. Each container runs an OpenClaw Gateway, and Claude Sonnet 4 operates as a private AI for that user alone.

---

## 1. The App: Tauri 2 + Next.js

The app is built with **Tauri 2**. I chose it over React Native or Flutter for three reasons.

**Cross-platform from day one.** Tauri isn't just Android. A single codebase targets **iOS, Android, Windows, macOS, Linux, and web**. Jiyul was never meant to be locked into one platform, so Tauri was the natural fit.

**Reuse the Next.js codebase.** The UI was already in Next.js. Tauri just wraps it with a Rust native layer — no new paradigm to learn, unlike React Native.

**Performance.** Unlike Electron, Tauri doesn't bundle Chromium. It uses the OS's built-in WebView, which means **smaller app size and better memory efficiency** — especially noticeable on mobile.

```
Framework:  Tauri 2 (Rust + OS WebView)
UI:         Next.js 14 (SSG mode)
Languages:  TypeScript + Kotlin/Swift + Rust
Targets:    Android / iOS / Desktop / Web — single codebase
```

One gotcha: **targetSdk 35+ forces edge-to-edge layout** on Android, breaking the safe area (Tauri #14142). Unresolved upstream issue — currently working around it with targetSdk 34.

---

## 2. Auth: Firebase + Android CredentialManager

Authentication is **Firebase Auth + Google Sign-In**. Modern Android recommends the **CredentialManager API** over the legacy Google Sign-In SDK.

```kotlin
val credentialManager = CredentialManager.create(context)
val request = GetCredentialRequest(listOf(
    GetGoogleIdOption(serverClientId = WEB_CLIENT_ID, ...)
))
val result = credentialManager.getCredential(context, request)
// → ID Token → Firebase signInWithCredential
```

Once Firebase hands back a UID, it tags every subsequent request. The UID doubles as the container identifier.

---

## 3. Container Manager: Per-User Docker Lifecycle

The backbone of the architecture is a **Container Manager** written in Python. It runs as an HTTP server on port 19200, takes Firebase UIDs, and manages the Docker container lifecycle for each user.

### API

| Method | Path | Purpose |
|--------|------|---------|
| `POST` | `/ensure` | Create/start container → return port + token |
| `POST` | `/proxy/{uid}/v1/*` | Proxy request to user's container |
| `GET` | `/health` | Manager health check |

### Lifecycle

```python
# /ensure endpoint — pseudocode
def ensure_container(uid, email, display_name):
    container_name = f"jiyul-{uid[:8]}"

    if not container_exists(container_name):
        docker.run(
            image="jiyul-openclaw:latest",
            name=container_name,
            volumes=[
                f"./data/workspaces/{uid}:/home/jiyul/workspace",  # read-write
                f"./skills:/home/jiyul/skills:ro",                 # read-only
            ],
            mem_limit="2g",
            cpus=2.0,
            restart_policy="unless-stopped"
        )
        write_user_md(uid, email, display_name)

    elif is_stopped(container_name):
        docker.start(container_name)
        time.sleep(5)  # wait for Gateway to boot

    return {"port": get_port(container_name), "token": get_token(uid)}
```

**Containers auto-stop after 30 minutes of idle.** A check loop runs every 5 minutes. The next request triggers `/ensure`, which starts the container back up. `--restart unless-stopped` handles server reboots automatically.

This means **only active containers consume resources**, regardless of how many users are registered.

---

## 4. Inside the Container: User Data vs. Domain Knowledge

Each Docker container runs an **OpenClaw Gateway** — an OpenAI-compatible API interface that routes to Claude Sonnet 4 under the hood.

Volumes are split by purpose:

```
/home/jiyul/
├── workspace/              # per-user persistent volume (read-write)
│   ├── SOUL.md            # Jiyul's persona definition
│   ├── IDENTITY.md        # Jiyul's identity
│   ├── AGENTS.md          # agent configuration
│   └── USER.md            # user info (auto-generated)
├── skills/                 # domain expertise (read-only mount)
│   ├── tax-law/           # tax code summaries, deadlines, amendment history
│   ├── civil-law/         # key civil/commercial law provisions
│   └── case-references/   # key case rulings
└── .openclaw-jiyul/
    └── openclaw.json      # Gateway config + API key
```

```bash
docker run \
  -v ./data/workspaces/${uid}:/home/jiyul/workspace \
  -v ./skills:/home/jiyul/skills:ro \
  --memory=2g --cpus=2 \
  jiyul-openclaw:latest
```

**User data (workspace/) is read-write. Domain knowledge (skills/) is read-only.** That separation is the point.

When the tax code changes or a new ruling drops, I update files in `skills/` on the host — **no container rebuild needed, changes propagate instantly to every user**. And since users can't modify or delete the skills directory, the knowledge base stays intact.

---

## 5. File Attachments: Cloudflare R2 Workers

Contract PDFs and images go through **Cloudflare R2 + Workers**.

```
App → POST /upload (Worker) → store in R2 → return URL
App → GET /files/:key (Worker) → serve from R2
```

The Worker handles auth in the middle. Files are served from CDN edge nodes, so latency is low. One thing to watch: **R2 TTL isn't automatic** — you need to set lifecycle rules manually in the Cloudflare dashboard.

---

## 6. Cloudflare Tunnel: Deploy Without Opening Ports

Instead of exposing the server port directly, I use **Cloudflare Tunnel**.

```yaml
# config-jiyul.yml
tunnel: cc32d1d4-807a-46e8-a198-58724a8764c2
ingress:
  - hostname: api.tax-insight.kr
    service: http://localhost:19200
```

No inbound firewall rules. `cloudflared` punches an outbound tunnel to the Cloudflare edge. A `@reboot` crontab entry handles restarts. DDoS protection, SSL termination, and CDN come along for free.

---

## 7. Conversation Storage: Firestore

Conversation history lives in **Firestore**, not in the container. That's why conversations survive container stop/start cycles.

```
conversations/{uid}/
  messages/{messageId}
    role: "user" | "assistant"
    content: "..."
    timestamp: ...
```

Security rules lock each user's data to their own UID.

---

## Stack Summary

| Layer | Technology |
|-------|------------|
| App framework | Tauri 2 + Next.js 14 (SSG) |
| Languages | TypeScript, Kotlin/Swift, Rust |
| Auth | Firebase Auth + Android CredentialManager |
| Database | Firestore |
| AI | OpenClaw + Claude Sonnet 4 |
| Containers | Docker (node:22-slim) |
| Domain knowledge | read-only volume mount (skills/) |
| CDN / Tunnel | Cloudflare Tunnel + R2 Workers |
| Domain | api.tax-insight.kr |

---

## What I Learned

**1. Tauri still has edge cases on Android.**
The targetSdk issue is a good example — upstream bugs you can't fix. The tradeoff of WebView-based apps.

**2. Per-user container isolation is simpler than it looks.**
The Container Manager is around 200 lines. The idle stop + on-demand start pattern keeps costs surprisingly manageable.

**3. Read-only volumes are what make domain AI trustworthy.**
Separating user data from the knowledge base means you can update knowledge fast without touching user data. No container rebuild for a tax code amendment.

**4. Cloudflare Tunnel is a cheat code for small deployments.**
Port forwarding, SSL certificates, DDoS protection — solved in one config file. The free tier is enough.

**5. Manage persona as a file.**
One `SOUL.md` defines the AI's personality, domain scope, and tone. Change the text file, no code change needed.

---

## Closing

Jiyul is still early. A small group of users is quietly testing it now. Next up: legal document analysis, case law search integration, and tax filing assistance.

The technology stack wasn't the hard part. The real question was: **"How do you isolate a domain-specialized AI safely, and manage its knowledge in a way you can actually trust?"**

User data as read-write for personalization. Domain knowledge as read-only for integrity. That simple principle might be the right starting point for enterprise AI services.

---

*Jiyul (知律) | AI Legal & Tax Assistant | tax-insight.kr*

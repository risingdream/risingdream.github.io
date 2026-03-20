---
title: "유저별 AI 컨테이너를 Docker로 분리한 법: 지율(知律) 개발기"
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

> Tauri 2 + Next.js + OpenClaw + Docker로 만든 AI 법률·세무 비서 앱

---

## 왜 만들었나

OpenClaw는 강력하다. 유저별 독립 워크스페이스, 페르소나 정의, 툴 연동, 메모리 — 개인 AI 에이전트로서 필요한 건 다 있다.

근데 이걸 **서비스로 팔려면** 얘기가 달라진다.

세 가지가 부족했다.

**첫째, 엔터프라이즈 안정성.** 내 로컬에서 OpenClaw 띄우는 건 쉽다. 근데 모르는 유저 1000명이 동시에 쓴다면? 컨테이너가 죽으면 자동 복구되나? 리소스 폭주하면 제한이 걸리나? 유저 데이터가 격리되나? 이 질문들에 "네"라고 답할 수 있어야 서비스다.

**둘째, 도메인 전문성.** 범용 AI는 법률·세무 질문에 절반만 맞는다. 세법 조항을 틀리거나, 최신 개정 내용을 모르거나, 엉뚱한 자신감으로 대답한다. 지율은 두 가지로 이 문제를 푼다. 하나는 `SOUL.md`로 주입하는 페르소나 — 법률·세무 전문가로서의 말투, 판단 기준, 한계 인식. 다른 하나는 **도메인 전문 스킬을 Docker 컨테이너에 read-only로 마운트**하는 방식이다. 세법 조문 요약, 판례 요지, 신고 기한 캘린더 같은 지식 베이스를 컨테이너 내부에 파일로 올려두고, AI가 이를 참조한다. 유저가 건드릴 수 없는 read-only 영역이라 **지식의 무결성이 보장**된다.

**셋째, 상품화 구조.** 유저 온보딩, 인증, 데이터 저장, 파일 첨부 — OpenClaw 자체엔 이런 프로덕트 레이어가 없다. 이걸 붙여야 "서비스"가 된다.

**지율(知律)은 그 세 가지를 붙인 실험이다.**

OpenClaw의 철학 — *유저마다 독립된 AI 환경, 지속되는 맥락, 커스터마이즈 가능한 페르소나* — 은 그대로 유지한다. 여기에 Docker 기반 컨테이너 격리로 엔터프라이즈 안정성을 얹고, 법률·세무 도메인 전문성을 `SOUL.md` 한 장과 read-only 스킬 볼륨으로 주입한다.

결국 질문은 이거다: **"OpenClaw를 SaaS로 만들면 어떻게 생겼을까?"**

지율은 그 답의 첫 번째 버전이다.

---

## 전체 아키텍처

```
Android App (Tauri 2 + Next.js)
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
(유저 A)    (유저 B)
OpenClaw    OpenClaw
Gateway     Gateway
```

핵심은 **유저 1명 = Docker 컨테이너 1개**라는 원칙이다. 각 컨테이너 안에 OpenClaw Gateway가 뜨고, Claude Sonnet 4가 그 유저만을 위한 AI로 작동한다.

---

## 1. 앱: Tauri 2 + Next.js

모바일 앱은 **Tauri 2**로 만들었다. React Native나 Flutter 대신 Tauri를 선택한 이유는 세 가지다.

**하나, 크로스플랫폼.** Tauri는 Android 하나가 아니다. 코드베이스 하나로 **iOS, Android, Windows, macOS, Linux, 웹**까지 커버된다. 지율을 앱으로만 묶어두지 않겠다는 전제가 있었기 때문에 Tauri는 자연스러운 선택이었다.

**둘, Next.js 코드베이스 재활용.** 이미 Next.js로 UI를 만들고 있었다. Tauri는 그 위에 Rust 네이티브 레이어만 얹으면 된다. React Native처럼 새 패러다임을 배울 필요가 없다.

**셋, 성능.** Tauri는 Electron과 달리 Chromium을 번들하지 않는다. OS 내장 WebView를 그대로 쓰기 때문에 **앱 용량이 작고 메모리 효율이 높다**. 특히 모바일에서 체감 차이가 크다.

```
앱 프레임워크: Tauri 2 (Rust + OS 내장 WebView)
UI: Next.js 14 (SSG 모드)
언어: TypeScript + Kotlin/Swift + Rust
타겟: Android / iOS / Desktop / Web — 단일 코드베이스
```

한 가지 함정이 있었다. **targetSdk 35+에서 edge-to-edge가 강제 적용**되면서 safe area 레이아웃이 깨지는 문제(Tauri #14142). 아직 미해결 이슈라 Android는 현재 targetSdk 34로 우회 중이다.

---

## 2. 인증: Firebase + Android CredentialManager

인증은 **Firebase Auth + Google Sign-In** 조합이다. 요즘 Android는 구버전 Google Sign-In SDK 대신 **CredentialManager API** 사용을 권장한다.

```kotlin
// Android 네이티브 CredentialManager
val credentialManager = CredentialManager.create(context)
val request = GetCredentialRequest(listOf(
    GetGoogleIdOption(serverClientId = WEB_CLIENT_ID, ...)
))
val result = credentialManager.getCredential(context, request)
// → ID Token → Firebase signInWithCredential
```

Firebase에서 UID를 받으면, 그때부터 모든 요청에 그 UID가 따라다닌다. 컨테이너 식별자 역할을 하는 것이다.

---

## 3. Container Manager: 유저별 Docker 생명주기

아키텍처의 핵심은 Python으로 만든 **Container Manager**다. 포트 19200에서 HTTP 서버로 돌고, Firebase UID를 받아서 해당 유저의 Docker 컨테이너를 관리한다.

### API 구조

| Method | Path | 역할 |
|--------|------|------|
| `POST` | `/ensure` | 컨테이너 생성/시작, 포트+토큰 반환 |
| `POST` | `/proxy/{uid}/v1/*` | 해당 유저 컨테이너로 요청 프록시 |
| `GET` | `/health` | 매니저 상태 확인 |

### 컨테이너 생명주기

```python
# /ensure 엔드포인트 핵심 로직 (의사 코드)
def ensure_container(uid, email, display_name):
    container_name = f"jiyul-{uid[:8]}"

    if not container_exists(container_name):
        # 최초 생성: 워크스페이스 + 스킬 볼륨 마운트
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
        time.sleep(5)  # Gateway 기동 대기

    return {"port": get_port(container_name), "token": get_token(uid)}
```

**idle 30분이 지나면 자동 stop**한다. 5분마다 체크 루프가 돌면서 마지막 요청 시각을 확인한다. 다음 요청이 오면 `/ensure`가 자동으로 다시 start시킨다. 서버 재부팅 시엔 `--restart unless-stopped` 덕분에 자동 복구된다.

이 구조 덕분에 유저가 늘어나도 **실제로 활성화된 컨테이너만 리소스를 쓴다**.

---

## 4. 컨테이너 내부: 유저 데이터 vs 도메인 지식 분리

각 Docker 컨테이너 안에는 **OpenClaw Gateway**가 떠있다. OpenAI API 호환 인터페이스를 제공하면서, 내부에서 Claude Sonnet 4를 호출한다.

볼륨은 목적에 따라 두 개로 분리된다.

```
/home/jiyul/
├── workspace/              # 유저별 퍼시스턴트 볼륨 (read-write)
│   ├── SOUL.md            # 지율이 페르소나 정의
│   ├── IDENTITY.md        # 지율이 정체성
│   ├── AGENTS.md          # 에이전트 설정
│   └── USER.md            # 유저 정보 (자동 생성)
├── skills/                 # 도메인 전문 스킬 (read-only 마운트)
│   ├── tax-law/           # 세법 조문 요약, 신고 기한, 개정 이력
│   ├── civil-law/         # 민법·상법 핵심 조항
│   └── case-references/   # 주요 판례 요지
└── .openclaw-jiyul/
    └── openclaw.json      # Gateway 설정 + API 키
```

```bash
docker run \
  -v ./data/workspaces/${uid}:/home/jiyul/workspace \
  -v ./skills:/home/jiyul/skills:ro \
  --memory=2g --cpus=2 \
  jiyul-openclaw:latest
```

**유저 데이터(workspace/)는 read-write, 도메인 지식(skills/)은 read-only.** 이 분리가 핵심이다.

법 개정이나 판례 업데이트가 생기면 **컨테이너를 재빌드하지 않고 호스트의 `skills/` 파일만 수정**하면 된다. 모든 유저 컨테이너에 즉시 반영된다. 유저가 skills 디렉토리를 수정하거나 삭제할 수 없으니 **지식 베이스의 무결성도 보장**된다.

---

## 5. 파일 첨부: Cloudflare R2 Workers

계약서 PDF나 이미지를 첨부할 때는 **Cloudflare R2 + Workers**를 쓴다.

```
앱 → POST /upload (Worker) → R2 저장 → URL 반환
앱 → GET /files/:key (Worker) → R2 서빙
```

Workers가 중간에서 인증을 처리하고, R2에 파일을 올린다. CDN 엣지에서 서빙되니까 속도도 빠르다. 주의할 점은 **R2 TTL 설정을 Cloudflare 대시보드에서 수동으로 해줘야** 한다는 것 — lifecycle rule이 자동 적용되지 않는다.

---

## 6. Cloudflare Tunnel: 포트 열지 않는 배포

서버 포트를 외부에 직접 노출하는 대신 **Cloudflare Tunnel**을 쓴다.

```yaml
# config-jiyul.yml
tunnel: cc32d1d4-807a-46e8-a198-58724a8764c2
ingress:
  - hostname: api.tax-insight.kr
    service: http://localhost:19200
```

방화벽 인바운드 규칙 없이, `cloudflared`가 Cloudflare 엣지로 아웃바운드 터널을 뚫는다. `@reboot` 크론탭으로 서버 재시작 시 자동 복구된다. DDoS 방어, SSL 종료, CDN이 덤으로 따라온다.

---

## 7. 대화 저장: Firestore

대화 히스토리는 컨테이너가 아니라 **Firestore**에 저장한다. 컨테이너가 stop됐다가 다시 start돼도 대화가 복원되는 이유다.

```
Firestore 구조:
conversations/{uid}/
  messages/{messageId}
    role: "user" | "assistant"
    content: "..."
    timestamp: ...
```

보안 규칙으로 본인 UID의 데이터만 읽고 쓸 수 있도록 잠가뒀다.

---

## 기술 스택 요약

| 영역 | 기술 |
|------|------|
| 앱 프레임워크 | Tauri 2 + Next.js 14 (SSG) |
| 언어 | TypeScript, Kotlin/Swift, Rust |
| 인증 | Firebase Auth + Android CredentialManager |
| DB | Firestore |
| AI | OpenClaw + Claude Sonnet 4 |
| 컨테이너 | Docker (node:22-slim) |
| 도메인 지식 | read-only 볼륨 마운트 (skills/) |
| CDN/터널 | Cloudflare Tunnel + R2 Workers |
| 도메인 | api.tax-insight.kr |

---

## 배운 것들

**1. Tauri는 아직 Android에서 엣지 케이스가 많다.**
targetSdk 이슈처럼 upstream이 해결 안 된 버그가 있다. 웹뷰 기반 앱의 숙명이다.

**2. 유저별 컨테이너 분리는 생각보다 단순하다.**
복잡해 보이지만 Container Manager 로직은 200줄 남짓이다. idle stop + on-demand start 패턴이 비용 효율도 좋다.

**3. read-only 볼륨이 도메인 AI의 신뢰성을 만든다.**
유저 데이터와 지식 베이스를 분리하면, 지식 업데이트는 빠르게, 유저 데이터는 안전하게 관리할 수 있다. 컨테이너 재빌드 없이 세법 개정 반영이 가능하다.

**4. Cloudflare Tunnel은 소규모 배포의 치트키다.**
포트 포워딩, SSL 인증서, DDoS 방어를 한 방에 해결한다. 무료 플랜으로도 충분하다.

**5. 페르소나는 파일로 관리하라.**
`SOUL.md` 하나로 AI의 성격, 전문 분야, 말투를 정의한다. 코드 변경 없이 텍스트 파일만 수정하면 된다.

---

## 마치며

지율은 아직 초기다. 현재 소수 유저로 조용히 테스트 중이다. 앞으로 법률 문서 분석, 판례 검색 연동, 세무 신고 보조 기능을 붙여나갈 예정이다.

이 프로젝트에서 가장 중요하게 생각한 건 기술 스택이 아니다. **"도메인 전문 AI를 어떻게 안전하게 격리하고, 지식을 신뢰할 수 있게 관리할 것인가"** — 이 질문에 대한 구조적 답이었다.

유저 데이터는 read-write로 개인화하고, 도메인 지식은 read-only로 무결성을 지킨다. 이 단순한 원칙이 엔터프라이즈 AI 서비스의 출발점이 될 수 있다고 생각한다.

---

*지율(知律) | AI 법률·세무 비서 | tax-insight.kr*

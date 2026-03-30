---
title: "Why Korean Online Banking Became a Security Galapagos"
date: 2026-03-30
categories:
  - Tech & AI
tags:
  - fintech
  - security
  - regulation
  - Korea
  - passkeys
  - FIDO
excerpt: "How a 1999 law designed to protect online banking created a closed ecosystem that made security worse — and left Korea behind as the world moved to passkeys."
---

In the early 2000s, logging into a Korean bank online required a small ordeal. A cascade of installation prompts would appear. You had to allow ActiveX controls, obtain a government-issued digital certificate, and save it to a USB stick or your desktop. If you weren't using Internet Explorer on Windows, you simply couldn't proceed. At the time, all of this was considered the mark of a secure system.

Two decades later, the certificate mandates are gone and ActiveX is dead. Yet people setting up a Korean banking app for the first time still ask the same question: why is this so complicated? In some ways, it has gotten stranger.

## 1999: A Reasonable Choice That Became a Trap

Korea's public key infrastructure was born out of the Electronic Signature Act of 1999. Coming off the shock of the Asian financial crisis, the government needed to get e-commerce and internet banking off the ground quickly. The question of how to trust an online transaction demanded an answer, and PKI with government-issued certificates was a reasonable one given the technology of the time.

The problem was making it mandatory by law. Financial regulators required banks to use these certificates for all internet banking transactions. That regulatory mandate locked the entire ecosystem into a specific technology stack. Certificates needed ActiveX to run in a browser. ActiveX only worked in Internet Explorer. A regulation intended to ensure security had instead chosen a technology — and the consequences would last for decades.

## The ActiveX Kingdom and the Interests That Kept It Alive

By the mid-2000s, Korean internet banking was effectively an IE-and-Windows-only experience. Mac and Linux users were officially excluded. That this persisted for over a decade was not simply technological inertia. A triangle of entrenched interests had no reason to change.

Banks could transfer authentication liability to customers: if a transaction was signed with the certificate, it was the customer's responsibility. Security vendors had steady revenue from selling ActiveX modules to every financial institution. Regulators, for whom the standard question after any incident was "did you follow the rules," found the status quo the safest posture. In this triangle, user experience was never the priority.

## The Global Fork in the Road

While Korea was locked in, the rest of the world chose a different path.

The United States moved toward layered fraud detection rather than mandated authentication technology. The model was: make it easy for users, and invest in back-end anomaly detection to catch what slips through.

Europe went further. PSD2 and its Strong Customer Authentication framework set security standards without specifying how to meet them. Any method that reached the required security level was acceptable. The regulation targeted outcomes, not implementations.

The contrast with Korea's approach is stark. Global regulators were **technology-neutral**; Korea's were **technology-specific**. One model allows standards to evolve as technology advances. The other freezes the ecosystem until the regulation itself is rewritten.

## After ActiveX: Things Got Worse

In 2020, the mandatory use of government certificates was finally abolished. ActiveX was gone. Problem solved?

When ActiveX was blocked in modern browsers, security vendors found a new approach: install a small application on the user's PC that runs a local web server, then have the banking site communicate with it over HTTP. The plugin was gone. The underlying structure was not.

The result was more complexity, not less.

A typical Korean banking site now requires five separate security applications before you can log in. Different sites require different combinations, so a typical Korean PC runs a dozen or more of these applications simultaneously — plus one more application to manage them all. There is no centralized update server; each bank's website is responsible for distributing and updating the software it depends on. When a vulnerability is found, getting a patch to users is nearly impossible by design.

Security researcher Wladimir Palant spent months analyzing these applications and found software quality that belonged to another era: code written in C, compiled with tools released fifteen years ago, with basic memory protections like ASLR and DEP disabled. Applications marketed as security tools, failing the most elementary security standards.

The danger was not theoretical. The North Korean Lazarus Group hacked the distribution servers of Veraport — the application used to manage this entire zoo of security software — and used it to deliver malware to millions of Korean PCs. The program installed to protect users became the attack vector.

One application, TouchEn nxKey, installed by nearly every Korean bank as a keyboard protection tool, was found to intercept all keystrokes — including those outside the browser — in a manner structurally identical to a keylogger. An anti-keylogging tool that works like a keylogger.

## The World Moved to Passkeys

While Korea was managing this increasingly tangled legacy, the rest of the world landed on something genuinely new.

Apple, Google, and Microsoft — three companies that rarely agree — converged on a single standard: **passkeys**, built on FIDO2 and WebAuthn. No passwords. No security software to install. No SMS codes. A cryptographic key stored on your device, unlocked with a fingerprint or face scan. Phishing-resistant by design, because there is no password to steal.

The numbers show how fast this has moved. More than one billion people have activated a passkey (FIDO Alliance, 2025). Eight of the top ten websites on the internet now support them. Google has over 800 million accounts using passkeys. Amazon enrolled 175 million users in the first year. Microsoft reports a 98% login success rate with passkeys, compared to 32% for passwords. Login speeds are 6x faster on Amazon, 17x faster on TikTok.

Financial institutions are not lagging behind. Spain's ABANCA bank reports that 42% of mobile banking customers now authorize transactions with passkeys. Digital banks like Revolut and Ubank have moved entirely to passkey login. Australia's government services platform saw 20,000 users enroll passkeys in the first week. The US NIST updated its 2025 guidelines to mandate phishing-resistant authentication — WebAuthn and FIDO2 — for all federal agencies.

In Korea, Kakao has adopted passkeys. Major banks and brokerages are described as having "begun piloting in 2025." A parliamentary audit raised the point that Taiwan had implemented passkey-based protections immediately after a data breach, while Korea's response was inadequate.

This is not a gap in speed. It is a gap in direction.

## The Galapagos Lesson

The animals of the Galapagos Islands evolved in extraordinary ways because they were isolated. Their ecosystem was complete and self-consistent. But it was disconnected from the rest of the world.

Korean financial security followed the same pattern. A rational choice in 1999 became a regulatory lock-in. The entrenched interests of banks, vendors, and regulators kept the ecosystem sealed. When ActiveX fell, something more fragmented and arguably more dangerous took its place. And while all of this was happening, the world converged on a standard that is faster, safer, and simpler.

The lesson is not that Korea made uniquely bad decisions. It is that when a regulation specifies a technology rather than an outcome, the regulation ages with the technology. The alternative — setting security standards and letting the market find the best implementation — is what allowed passkeys to emerge and spread globally in just a few years.

Kakao Bank and Toss proved that security and convenience are not a trade-off. They showed that a different approach could meet the same bar. That was the beginning. The question now is how quickly the rest of Korean banking joins the standard the rest of the world has already adopted.

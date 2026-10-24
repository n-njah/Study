# OWASP Top 10 Study Routine — 8-Week Program

> A structured study plan tailored for a Cybersecurity Analyst / Penetration Tester role.

---

## ⚠️ Important Correction: Which Version Is Current?

The source document labelled this material "OWASP Top 10 2025," but the ten categories it listed — in that exact order — are the **OWASP Top 10:2021**. The 2025 edition was released in November 2025 and **reordered, renamed, and replaced several categories**. Studying the 2021 numbering while believing it is 2025 will leave gaps in coverage.

Per the OWASP Foundation, "the most current released version is the OWASP Top 10 2025" ([owasp.org/projects/top-ten](https://owasp.org/projects/top-ten)).

### The Actual OWASP Top 10:2025

| ID | Category |
| --- | --- |
| **A01:2025** | Broken Access Control |
| **A02:2025** | Security Misconfiguration |
| **A03:2025** | Software Supply Chain Failures |
| **A04:2025** | Cryptographic Failures |
| **A05:2025** | Injection |
| **A06:2025** | Insecure Design |
| **A07:2025** | Authentication Failures |
| **A08:2025** | Software or Data Integrity Failures |
| **A09:2025** | Security Logging & Alerting Failures |
| **A10:2025** | Mishandling of Exceptional Conditions |

*Source: [OWASP Top 10:2025 Introduction](https://owasp.org/Top10/2025/0x00_2025-Introduction/)*

### What Changed from 2021

| 2021 Position | 2025 Position | Change |
| --- | --- | --- |
| A01 Broken Access Control | A01 | Same position; **SSRF (A10:2021) merged in** |
| A05 Security Misconfiguration | A02 | Moved up from #5 |
| A06 Vulnerable & Outdated Components | A03 | **Renamed and broadened** to Software Supply Chain Failures |
| A02 Cryptographic Failures | A04 | Moved down |
| A03 Injection | A05 | Moved down |
| A04 Insecure Design | A06 | Moved down |
| A07 Identification & Authentication Failures | A07 | Renamed to Authentication Failures |
| A08 Software & Data Integrity Failures | A08 | Same position |
| A09 Security Logging & Monitoring Failures | A09 | Renamed to Security Logging & Alerting Failures |
| A10 SSRF | — | **Removed**; CWE-918 consolidated under A01 |
| — | A10 | **New:** Mishandling of Exceptional Conditions |

Two new categories and one consolidation were introduced in 2025. Per OWASP, A01:2025 Broken Access Control remains at #1, with data indicating that on average 3.73% of applications tested had one or more of the 40 CWEs in that category. A02:2025 Security Misconfiguration rose from #5 in 2021 to #2, with 3.00% of tested applications showing one or more of its 16 CWEs.

Reporting on the release notes that **Software Supply Chain Failures** and **Mishandling of Exceptional Conditions** are the new entrants, with SSRF folded into Broken Access Control (Fastly; CSO Online).

> **Study implication:** If you have already memorised the 2021 list, the practical changes you must internalise are (a) SSRF is no longer a standalone category, (b) supply chain has displaced vulnerable-components, and (c) improper error handling / failing open is now its own category.

---

## 📋 Study Routine Overview

| Phase | Weeks | Focus |
| --- | --- | --- |
| **Phase 1** | 1–2 | Foundation building, framework mechanics, access control & crypto |
| **Phase 2** | 3–5 | Core vulnerability classes across all ten categories |
| **Phase 3** | 6–8 | API security, penetration-testing integration, emerging risks |

---

## Phase 1: Foundation Building (Weeks 1–2)

### Week 1: Understanding the Framework

- Read the official OWASP Top 10 documentation at <https://owasp.org/projects/top-ten>
- Read the [2025 Introduction](https://owasp.org/Top10/2025/0x00_2025-Introduction/) specifically to understand what changed and why
- Understand the data collection methodology and risk rating system
- Learn how OWASP calculates severity scores (CVSS integration)
- Set up a lab environment: DVWA, OWASP Juice Shop, or PortSwigger Web Security Academy

### Week 2: Authorization & Cryptography Deep Dive

- **A01:2025 — Broken Access Control**
  - IDOR, privilege escalation, horizontal/vertical access control bypass
  - **SSRF (CWE-918)** — now lives here, so study internal-network pivoting alongside access control
  - Deny-by-default enforcement, record-ownership modelling, directory listing exposure
- **A04:2025 — Cryptographic Failures**
  - TLS misconfigurations, weak algorithms, improper key management
  - CWE-200, CWE-327, CWE-798
- **Hands-on:** Practise exploiting broken access controls in your test environment

---

## Phase 2: Core Vulnerabilities (Weeks 3–5)

### Week 3: Injection & Security Misconfiguration

- **A05:2025 — Injection**
  - SQL, NoSQL, OS command, and LDAP injection variants
  - Relevant CWEs include CWE-78, CWE-79, CWE-89, CWE-94
- **A02:2025 — Security Misconfiguration**
  - Default credentials, verbose errors, unnecessary features enabled
  - Insecure defaults, permissive cloud configuration, missing security headers
  - Hardening processes, removing unused features, segmented architecture
- **Hands-on:** Build injection payloads and scan for misconfigurations

### Week 4: Supply Chain & Authentication Failures

- **A03:2025 — Software Supply Chain Failures**
  - CVE tracking, dependency scanning (Snyk, Dependabot)
  - Third-party packages, build systems, update channels
  - Broader in scope than the former "Vulnerable and Outdated Components"
- **A07:2025 — Authentication Failures**
  - Session management, MFA bypass, credential stuffing
- **Hands-on:** Component vulnerability assessment exercises

### Week 5: Design, Integrity, Logging & Exceptional Conditions

- **A06:2025 — Insecure Design**
  - Threat modelling, business logic flaws, architectural weaknesses
- **A08:2025 — Software or Data Integrity Failures**
  - CI/CD pipeline compromises, unsigned updates
- **A09:2025 — Security Logging & Alerting Failures**
  - Detection gaps, alert fatigue, insufficient logging coverage
- **A10:2025 — Mishandling of Exceptional Conditions**
  - Improper error handling, failing open, unhandled edge cases
  - Resource exhaustion and state-corruption scenarios

---

## Phase 3: Advanced & Specialized Topics (Weeks 6–8)

### Week 6: API Security Extension

APIs are critical in modern architectures and are covered by a separate OWASP document. Study the **OWASP API Security Top 10 (2023)**:

- API1:2023 — Broken Object Level Authorization
- API2:2023 — Broken Authentication
- API3:2023 — Broken Object Property Level Authorization
- API7:2023 — Server-Side Request Forgery
- API9:2023 — Improper Inventory Management
- API10:2023 — Unsafe Consumption of APIs

### Week 7: Practical Penetration Testing Integration

- Map OWASP Top 10 findings to PTES (Penetration Testing Execution Standard)
- Create custom Burp Suite extensions for OWASP-specific testing
- Develop automated checklists for each vulnerability class
- Practise writing professional remediation reports

### Week 8: Emerging Risks & LLM Security

- Research the **OWASP Top 10 for LLM Applications** (separate OWASP project)
- Study AI/ML attack vectors: prompt injection, model poisoning
- Review the **OWASP Mobile Top 10 (2024)** if you test mobile applications
- Compile personal cheat sheets and reference materials

---

## 🔬 Recommended Resources

| Resource | Purpose | Link |
| --- | --- | --- |
| **Official OWASP Site** | Primary documentation | `owasp.org/projects/top-ten` |
| **PortSwigger Web Security Academy** | Hands-on labs | `portswigger.net/web-security` |
| **OWASP Juice Shop** | Practice application | `owasp.org/www-project-juice-shop` |
| **HackTheBox / TryHackMe** | CTF-style practice | `hackthebox.eu` / `tryhackme.com` |
| **SANS Reading Room** | Research papers | `sans.org/reading-room` |

---

## ⏰ Weekly Time Commitment

| Activity | Hours / Week | Total (8 weeks) |
| --- | --- | --- |
| Reading & Documentation | 3–4 | ~28 hours |
| Hands-on Lab Practice | 5–6 | ~42 hours |
| Tool Configuration & Scripting | 2–3 | ~18 hours |
| Review & Note Consolidation | 2 | ~14 hours |
| **Total** | **12–15 hrs/week** | **~102 hours** |

---

## 🎯 Deliverables for Each Week

1. **Weekly notes** summarising key vulnerabilities and exploitation techniques
2. **Proof-of-concept demonstrations** in your isolated lab
3. **Remediation recommendations** written for technical stakeholders
4. **Tool/scripts** created for automated detection (where applicable)

---

## 🛠️ Efficiency Recommendations Based on Your Role

Given your work with **LoRaWAN, ESP32, and RF technologies**:

1. **Add an IoT Security Layer** — Complement the OWASP Web Top 10 with the **OWASP IoT Top 10** and **OWASP Mobile Top 10**. Many embedded systems expose web interfaces vulnerable to these same issues.
2. **Hardware-in-the-Loop Testing** — Include physical device testing where firmware exposes web APIs or management interfaces.
3. **Automate Scanning** — Create scripts that integrate OWASP ZAP or Burp Suite into your RF/embedded testing workflow.
4. **Document Physical-Digital Attack Chains** — Many IoT breaches follow a path of web vulnerability (OWASP Top 10) → device compromise → RF/network pivoting.

---

## Sources

- [OWASP Top 10:2025](https://owasp.org/Top10/2025/) — official category list
- [OWASP Top 10:2025 Introduction](https://owasp.org/Top10/2025/0x00_2025-Introduction/) — what changed and why
- [OWASP Foundation — Top 10 project page](https://owasp.org/projects/top-ten) — version status
- Fastly, ["The New 2025 OWASP Top 10 List: What Changed"](https://www.fastly.com/blog/new-2025-owasp-top-10-list-what-changed-what-you-need-to-know)
- CSO Online, ["Software supply chain risks join the OWASP top 10 list"](https://www.csoonline.com/article/4126865/software-supply-chain-risks-join-the-owasp-top-10-list-access-control-still-on-top.html)

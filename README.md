# Hi, I'm David 👋

**🇬🇧 English Version** | **[🇩🇪 Deutsche Version](README_DE.md)**

---

**Full-stack development and operations** | One-person company with paying customers

12+ years in customer-facing technical roles (Amazon Digital Services, DKB Bank), now certified in IT administration and cloud infrastructure. Under **MelucioLabs** I build and run my own platform and the infrastructure it sits on: development, deployment, monitoring, on-call and the customer conversation are all in the same pair of hands.

My B2B SaaS experience comes from running my own platform, not from employment. What that means in practice, including the gaps, is spelled out further down.

---

## 🎓 Certifications

| Certification | Completed |
|---|---|
| **IHK Cloud Business Expert** | June 2026 |
| **IHK IT-Administrator** | April 2026 |
| **IHK IT-Support-Specialist** | February 2026 |
| **LPI Linux Essentials** (85%+) | November 2025 |

*IHK = German Chambers of Industry and Commerce, the national body for vocational qualifications.*

---

## 🏢 MelucioLabs

Sole proprietorship in Brandenburg, Germany. Websites, hosting and a customer portal for small businesses. The source lives in a private monorepo; the results are publicly reachable.

### Customer portal (multi-tenant)

The core of the platform. React (Vite) frontend, Node.js/Express backend on SQLite, multi-tenant: each customer sees exactly the features they subscribed to.

The module catalogue currently holds **40+ bookable modules**, among them:

| Area | What it covers |
|---|---|
| **Appointment booking** | Public booking page, calendar subscription, waiting list, guest login via magic link, video appointments, travel radius |
| **Ticketing** | Own IMAP collection of customer mailboxes, folder rules, auto-reply, EML export |
| **Accounting** | Invoice generation with PDF output, recurring billing runs, German VAT pre-registration with ELSTER codes, payment handling, tax advisor export |
| **Others** | Shop, newsletter, contact management, mileage log, inventory, point of sale, events with shareable public pages, social media hub, encrypted cloud storage, glossary, Google reviews |

The catalogue is deliberately a single source in the backend. It used to be hard-coded in two frontends and drifted apart regularly; the recurring bug was always one forgotten place.

### Sites in production

| Domain | What it is |
|---|---|
| [meluciolabs.de](https://meluciolabs.de) | Own site, including an [engineering page](https://meluciolabs.de/engineering) |
| [kitchenhelper-ai.de](https://kitchenhelper-ai.de) | AI recipe generator with carbohydrate-unit calculation |
| [comavita.de](https://comavita.de) | Customer site with its own assistant |
| [t1d-spremberg.de](https://t1d-spremberg.de) | Type 1 diabetes support group, Spremberg |
| [t1d-nauen.de](https://t1d-nauen.de) | Type 1 diabetes support group, Nauen |
| [alte-brennerei-ribbeck.de](https://alte-brennerei-ribbeck.de) | Book series site including shop |
| [die-aeonischen.de](https://die-aeonischen.de) | Web demo for the series (Godot, WebAssembly) |

The static sites are Astro projects on Cloudflare Workers; the containerised applications run on a Hetzner server. Both are shipped through GitHub Actions.

### Security and operations

This is where most of the work sits, and the part I would rather talk about in an interview than the feature list.

- **Content Security Policy without `unsafe-inline`.** SHA-256 hashes of inline scripts computed at build time for statically served pages, a per-response nonce for server-side rendering. The trigger was one of my own scans: seven of eight sites had the identical hole, because a template had been copied seven times. `style-src 'unsafe-inline'` deliberately stays, because Astro emits dynamic inline styles and injected CSS is a different order of magnitude from injected JavaScript.
- **Purpose-built passive checkers** instead of guesswork: security headers and cookies, inline scripts the CSP will never execute, fixed filenames without a cache buster, PWA manifests including the update prompt, dead internal links and mixed content, link preview images, colour contrast in both themes.
- **Runs daily via cron, pushes only on state change.** A daily all-clear gets ignored within a week, and then the one message that matters goes down with it. Unchanged findings stay quiet, a reminder follows after seven days, and clearing them sends a low-priority all-clear.
- **Active probing only against my own domains**, enabled through a list rather than a flag. A flag eventually gets set by accident; a list does not.
- **SPF, DKIM and DMARC on every domain**, DMARC at `p=quarantine`, plus TLS-RPT and CAA. Reports land in an inbox that actually gets read.
- **A checker that reports its own blindness:** if detection stops matching after a refactor, it says so instead of staying silent. Otherwise that looks exactly like "all good", which is the class of failure these scripts exist for.

### AI integration

Five assistants in production, all Cloudflare Workers:

- **RAG on Cloudflare Vectorize**, a separate index per bot, fed by an ingest pipeline from curated Markdown knowledge files
- **Guardrails are deterministic, not prompt-based.** Anything a bot must not say (individual insulin dosing, target weight, firm price quotes) is caught by regex *before* the model. Repeatedly confirmed: a prompt prohibition loses against the model's training.
- **Streaming** over Server-Sent Events, so the answer arrives as it is written instead of appearing after ten seconds
- **Anonymous logging** to Cloudflare D1, nightly backup to R2, 90-day retention
- **Labelling per Art. 50 EU AI Act** in the header and below the input
- Conversation history stays in the device's `localStorage` and never leaves the browser

### Book project

A book series of my own, technically interesting because of the delivery path. EPUB and print-ready PDF are generated from Markdown manuscripts by script, sharing one layout source for the paperback and the shop edition. The shop does not serve a static file: for each purchase a Worker assembles a personalised copy with watermark and optional dedication from R2 masters. Payment runs through Stripe, redelivery is idempotent. The series is published on Amazon KDP in parallel.

---

## 🖥️ Infrastructure (24/7, self-hosted)

**Scale:** 30+ Docker containers across a Hetzner Cloud server (production) and a Raspberry Pi 5 (hot standby and dev)
**Stack:** Docker, Linux, Nextcloud, Pi-hole, MariaDB, PostgreSQL, SQLite
**Networking:** Cloudflare Tunnel for all public services, Tailscale VPN for remote access
**Delivery:** CI/CD via GitHub Actions (29 workflows), automated SSH deploy or Wrangler
**Monitoring:** Uptime Kuma with ntfy push, latency probe, certificate and chatbot watchdogs, deliverability digest
**Deploy rule:** build first, then swap. No `docker stop` before the build, otherwise a failed build takes the service down entirely instead of leaving the previous version running.

---

## 📦 Other public projects

#### 🎓 [linux-essentials-quiz](https://github.com/MCCMDave/linux-essentials-quiz)
Quiz system with 276 official Linux Essentials 010-160 exam questions, written while preparing for the exam I passed at 85%+. Three modes (learning/exam/custom), timed exam simulation. License: Apache 2.0

#### 📚 [Lernkarten-API](https://karten.meluciolabs.de)
Spaced-repetition flashcards using the Leitner algorithm, JWT auth, per-user progress, AI-assisted card extraction from PDF/TXT. Stack: FastAPI, SQLite

#### 🏠 [homelab-automation](https://github.com/MCCMDave/homelab-automation)
Monitoring and tracking tools for the production homelab, including power consumption with cost calculation

#### ☁️ [oci-instance-sniper](https://github.com/MCCMDave/oci-instance-sniper)
Automated Oracle Cloud ARM instance provisioning with retry logic, multi-region support, exponential backoff

#### 🔌 [ec2-connect](https://github.com/MCCMDave/ec2-connect)
AWS EC2 SSH connection manager for Windows, multi-instance, bilingual menu

#### 🪟 [windows-automation](https://github.com/MCCMDave/windows-automation)
Universal Update Manager for Windows (Chocolatey, winget, pip, npm), PowerShell-based

---

## 💻 Tech Stack

**Languages:** JavaScript/Node.js, Python, PowerShell, Bash, SQL
**Frontend:** React, Vite, Astro, Web Components, vanilla JS, PWA
**Backend:** Node.js/Express, FastAPI, SQLite, PostgreSQL, MariaDB
**Cloud:** Cloudflare (Workers, D1, R2, KV, Vectorize, Tunnel, Email Routing), Hetzner Cloud, Oracle Cloud, AWS basics
**Operations:** Docker, Linux, Nginx, GitHub Actions, Tailscale, Uptime Kuma, backup and restore procedures
**Practices:** Multi-tenant architecture, CSP and web security, GDPR and accessibility (WCAG/EU Accessibility Act), SEO, Stripe payments, mail deliverability, technical documentation

---

## 🎯 What I bring

**Development and operations in one pair of hands**
- A multi-tenant platform with paying customers, built and operated end to end
- Production infrastructure running 24/7, including backups, monitoring and recovery
- Security and deliverability treated as an ongoing routine, not a one-off project
- A strong documentation habit: runbooks, operating procedures, and written-down wrong turns

**Customer-facing background**
- 12+ years supporting customers on complex technical products
- Level-2 escalation specialist in a regulated banking environment (DKB), 95%+ first-contact resolution
- 8+ years at Amazon Digital Services (Prime Video, Echo, Kindle, Fire), 99% satisfaction
- Built a knowledge platform adopted by 20+ colleagues during the COVID crisis response

**Where the limits are**
- The platform is a one-person operation. I do not bring experience with larger engineering teams, team code-review culture, or Kubernetes in production.
- Scale here means a handful of tenants done properly, not thousands. Large-scale load problems I know from reading, not from running them.

---

## 📫 Contact

🌐 [meluciolabs.de/engineering](https://meluciolabs.de/engineering)
📧 bewerbungen@meluciolabs.de
📍 Brandenburg, Germany | 🌍 Open to remote

---

*Building reliable systems, helping people succeed* 🚀

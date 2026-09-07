# Hallo, ich bin David 👋

**[🇬🇧 English Version](README.md)** | **🇩🇪 Deutsche Version**

---

**Fullstack-Entwicklung und Betrieb** | Ein-Personen-Betrieb mit zahlenden Kunden

Über 12 Jahre in kundennahen technischen Rollen (Amazon Digital Services, DKB Bank), inzwischen zertifiziert in IT-Administration und Cloud-Infrastruktur. Unter **MelucioLabs** baue und betreibe ich eine eigene Plattform samt Infrastruktur: Entwicklung, Deployment, Monitoring, Rufbereitschaft und Kundengespräch liegen in derselben Hand.

B2B-SaaS-Erfahrung kommt bei mir aus der eigenen Plattform, nicht aus einer Anstellung. Was das bedeutet, steht weiter unten offen dabei.

---

## 🎓 Zertifikate

| Zertifikat | Abschluss |
|---|---|
| **IHK Cloud Business Expert** | Juni 2026 |
| **IHK IT-Administrator** | April 2026 |
| **IHK IT-Support-Specialist** | Februar 2026 |
| **LPI Linux Essentials** (85%+) | November 2025 |

---

## 🏢 MelucioLabs

Einzelunternehmen, Havelland. Websites, Betrieb und ein Kundenportal für kleine Betriebe. Der Quellcode liegt in einem privaten Monorepo, die Ergebnisse sind öffentlich erreichbar.

### Kundenportal (Multi-Tenant)

Kern der Plattform. React (Vite) im Frontend, Node.js/Express mit SQLite im Backend, mandantenfähig: Jeder Kunde sieht genau die Funktionen, die er gebucht hat.

Der Baustein-Katalog umfasst aktuell **über 40 buchbare Bausteine**, darunter:

| Bereich | Was drinsteckt |
|---|---|
| **Terminbuchung** | Öffentliche Buchungsseite, Kalender-Abo, Warteliste, Gast-Login per Magic-Link, Videotermine, Anfahrt und Einzugsgebiet |
| **Ticketsystem** | Eigene IMAP-Abholung der Kundenpostfächer, Ordnerregeln, Auto-Antwort, EML-Export |
| **Buchhaltung** | Rechnungserstellung mit PDF-Generierung, Abo- und Rechnungslauf, Umsatzsteuer-Voranmeldung mit ELSTER-Kennzahlen, Zahlungsabwicklung, Steuerberater-Export |
| **Weitere** | Shop, Newsletter, Kontaktverwaltung, Fahrtenbuch, Lagerverwaltung, Kasse, Events mit teilbaren Veranstaltungsseiten, Social-Media-Hub, verschlüsselter Cloud-Speicher, Glossar, Google-Reviews |

Der Katalog ist bewusst eine einzige Quelle im Backend. Vorher lag die Liste in zwei Frontends hartkodiert und lief regelmäßig auseinander; der wiederkehrende Fehler war jedes Mal eine vergessene Stelle.

### Betriebene Websites

| Domain | Was es ist |
|---|---|
| [meluciolabs.de](https://meluciolabs.de) | Eigener Auftritt inkl. [Engineering-Seite](https://meluciolabs.de/engineering) |
| [kitchenhelper-ai.de](https://kitchenhelper-ai.de) | KI-Rezeptgenerator mit BE-/KHE-Berechnung |
| [comavita.de](https://comavita.de) | Kundenseite mit eigenem Assistenten |
| [t1d-spremberg.de](https://t1d-spremberg.de) | Selbsthilfegruppe Diabetes Typ 1 Spremberg |
| [t1d-nauen.de](https://t1d-nauen.de) | Selbsthilfegruppe Diabetes Typ 1 Nauen |
| [alte-brennerei-ribbeck.de](https://alte-brennerei-ribbeck.de) | Buchreihe „Die Chroniken des Äonischen" inkl. Shop |
| [die-aeonischen.de](https://die-aeonischen.de) | Web-Demo zur Reihe (Godot, WebAssembly) |

Die statischen Seiten sind Astro-Projekte auf Cloudflare Workers, die Container-Anwendungen laufen auf einem Hetzner-Server. Ausgeliefert wird in beiden Fällen über GitHub Actions.

### Sicherheit und Betrieb

Der Teil, an dem am meisten Arbeit hängt, und der Teil, den ich am ehesten in einem Bewerbungsgespräch aufmache.

- **Content-Security-Policy ohne `unsafe-inline`.** Für statisch ausgelieferte Seiten SHA-256-Hashes der Inline-Skripte zur Bauzeit, bei serverseitigem Rendering ein Nonce je Antwort. Auslöser war ein eigener Scan: Sieben von acht Seiten hatten dieselbe Lücke, weil eine Vorlage siebenmal kopiert worden war. `style-src 'unsafe-inline'` bleibt bewusst stehen, weil Astro dynamische Inline-Styles erzeugt und eingeschleustes CSS eine andere Größenordnung ist als eingeschleustes JavaScript.
- **Eigene passive Prüfer** statt Bauchgefühl: Sicherheitskopfzeilen und Cookies, Inline-Skripte die die CSP nie ausführen lässt, feste Dateinamen ohne Cache-Buster, PWA-Manifeste samt Update-Hinweis, tote interne Links und Mixed Content, Vorschaubilder beim Teilen, Farbkontrast in beiden Themes.
- **Täglich per Cron, Push nur bei Zustandswechsel.** Eine tägliche Gut-Meldung wird nach einer Woche weggewischt, und dann geht die eine wichtige Meldung mit unter. Unveränderte Funde bleiben still, nach sieben Tagen kommt eine Erinnerung, bei Behebung eine leise Entwarnung.
- **Aktive Prüfungen nur gegen eigene Domains**, freigegeben über eine Liste und nicht über einen Schalter. Ein Schalter wird irgendwann versehentlich gesetzt, eine Liste nicht.
- **SPF, DKIM und DMARC auf allen Domains**, DMARC auf `p=quarantine`, dazu TLS-RPT und CAA. Berichte laufen in eine Inbox, in die wirklich geschaut wird.
- **Ein Prüfer, der seine eigene Blindheit meldet:** Greift die Erkennung nach einem Umbau ins Leere, gibt er einen Hinweis aus statt zu schweigen. Sonst sieht das exakt aus wie „alles in Ordnung", und das ist die Fehlerklasse, gegen die diese Skripte überhaupt gebaut sind.

### KI-Integration

Fünf Assistenten im Einsatz, alle als Cloudflare Worker:

- **RAG über Cloudflare Vectorize**, je Bot ein eigener Index, dazu eine Ingest-Strecke aus gepflegten Markdown-Wissensdateien
- **Grenzen deterministisch statt per Prompt.** Was ein Bot nicht sagen darf (individuelle Insulindosis, Zielgewicht, konkrete Preiszusagen), wird als Regex **vor** dem Modell abgefangen. Belegt: gegen die Prägung des Modells gewinnt ein Prompt-Verbot nicht.
- **Streaming** über Server-Sent Events, damit die Antwort mitläuft statt nach zehn Sekunden zu erscheinen
- **Anonymes Logging** in Cloudflare D1, nächtliche Sicherung nach R2, 90-Tage-Frist
- **Kennzeichnung nach Art. 50 EU AI Act** in Kopfzeile und unter der Eingabe
- Verlauf bleibt im `localStorage` des Geräts, nichts davon verlässt den Browser

### Buchprojekt

Eine eigene Buchreihe, technisch interessant wegen des Auslieferungswegs: EPUB und druckfertiges PDF entstehen per Skript aus Markdown-Manuskripten (gemeinsame Layout-Quelle für Taschenbuch und Shop-Fassung). Der Shop liefert keine statische Datei aus, sondern baut pro Kauf im Worker eine personalisierte Kopie mit Wasserzeichen und optionaler Widmung aus R2-Mastern; bezahlt wird über Stripe, die Wiederauslieferung ist idempotent. Parallel erscheint die Reihe bei Amazon KDP.

---

## 🖥️ Infrastruktur (24/7, selbst gehostet)

**Umfang:** 30+ Docker-Container auf einem Hetzner-Cloud-Server (Produktivsystem) und einem Raspberry Pi 5 (Hot-Standby und Entwicklung)
**Stack:** Docker, Linux, Nextcloud, Pi-hole, MariaDB, PostgreSQL, SQLite
**Netzwerk:** Cloudflare Tunnel für alle öffentlichen Dienste, Tailscale VPN für Remote-Zugriff
**Deployment:** CI/CD über GitHub Actions (29 Workflows), automatischer SSH-Deploy bzw. Wrangler
**Monitoring:** Uptime Kuma mit ntfy-Push, Latenz-Sonde, Zertifikats- und Chatbot-Wächter, Zustellbarkeits-Digest
**Grundregel beim Deploy:** erst bauen, dann tauschen. Kein `docker stop` vor dem Build, sonst ist der Dienst bei einem Fehlschlag komplett weg statt auf dem alten Stand.

---

## 📦 Weitere öffentliche Projekte

#### 🎓 [linux-essentials-quiz](https://github.com/MCCMDave/linux-essentials-quiz)
Interaktives Quizsystem mit 276 offiziellen Prüfungsfragen (Linux Essentials 010-160), entstanden bei der Vorbereitung auf die Prüfung, die ich mit 85%+ bestanden habe. Drei Modi (Lernen/Prüfung/Custom), Prüfungssimulation mit Timer. Lizenz: Apache 2.0

#### 📚 [Lernkarten-API](https://karten.meluciolabs.de)
Spaced-Repetition-Lernkarten nach dem Leitner-Algorithmus, JWT-Auth, nutzerspezifischer Fortschritt, KI-gestützte Kartenextraktion aus PDF/TXT. Stack: FastAPI, SQLite

#### 🏠 [homelab-automation](https://github.com/MCCMDave/homelab-automation)
Monitoring- und Tracking-Werkzeuge für das produktive Homelab, inklusive Stromverbrauch mit Kostenrechnung

#### ☁️ [oci-instance-sniper](https://github.com/MCCMDave/oci-instance-sniper)
Automatisiertes Provisioning von Oracle-Cloud-ARM-Instanzen mit Retry-Logik, Multi-Region, exponentielles Backoff

#### 🔌 [ec2-connect](https://github.com/MCCMDave/ec2-connect)
SSH-Verbindungsmanager für AWS EC2 unter Windows, Multi-Instance, zweisprachiges Menü

#### 🪟 [windows-automation](https://github.com/MCCMDave/windows-automation)
Universal Update Manager für Windows (Chocolatey, winget, pip, npm), PowerShell-basiert

---

## 💻 Tech Stack

**Sprachen:** JavaScript/Node.js, Python, PowerShell, Bash, SQL
**Frontend:** React, Vite, Astro, Web Components, Vanilla JS, PWA
**Backend:** Node.js/Express, FastAPI, SQLite, PostgreSQL, MariaDB
**Cloud:** Cloudflare (Workers, D1, R2, KV, Vectorize, Tunnel, Email Routing), Hetzner Cloud, Oracle Cloud, AWS-Grundlagen
**Betrieb:** Docker, Linux, Nginx, GitHub Actions, Tailscale, Uptime Kuma, Backup- und Restore-Strecken
**Praxis:** Multi-Tenant-Architektur, CSP und Web-Security, DSGVO und Barrierefreiheit (WCAG/BFSG), SEO, Stripe-Zahlungen, Mail-Zustellbarkeit, technische Dokumentation

---

## 🎯 Was ich mitbringe

**Betrieb und Entwicklung in einer Hand**
- Eine mandantenfähige Plattform mit zahlenden Kunden, eigenverantwortlich gebaut und betrieben
- Produktive Infrastruktur im 24/7-Betrieb, inklusive Backups, Monitoring und Wiederanlauf
- Sicherheits- und Zustellbarkeitspraxis als laufende Routine, nicht als einmaliges Projekt
- Ausgeprägte Dokumentationspraxis (Runbooks, Betriebsanleitungen, dokumentierte Fehlentscheidungen)

**Kundennähe**
- Über 12 Jahre Support bei komplexen technischen Produkten
- Level-2-Eskalationsspezialist im regulierten Bankenumfeld (DKB), über 95% Lösungsquote beim Erstkontakt
- 8+ Jahre Amazon Digital Services (Prime Video, Echo, Kindle, Fire), 99% Zufriedenheit
- Wissensplattform aufgebaut, die während der Pandemie von 20+ Kolleginnen und Kollegen genutzt wurde

**Wo die Grenzen liegen**
- Die Plattform ist ein Ein-Personen-Betrieb. Erfahrung mit größeren Entwicklungsteams, Code-Review-Kultur im Team und Kubernetes im Produktivbetrieb bringe ich nicht mit.
- Skalierung heißt hier: wenige Mandanten sauber, nicht Tausende. Lastprobleme im großen Maßstab kenne ich aus Literatur, nicht aus dem eigenen Betrieb.

---

## 📫 Kontakt

🌐 [meluciolabs.de/engineering](https://meluciolabs.de/engineering)
📧 bewerbungen@meluciolabs.de
📍 Havelland, Brandenburg | 🌍 Offen für Remote

---

*Verlässliche Systeme bauen, Menschen weiterhelfen* 🚀

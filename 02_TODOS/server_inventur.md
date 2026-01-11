# Server Inventur - Stand 2026-01-06

## Alle Projekte im Überblick

| Server | IP | Projekt | Status | Sessions |
|--------|-----|---------|--------|----------|
| SYSTEMHAUS-001 | .15 | **EVY/MIRA** - KI-Assistenten SaaS | PRODUKTIV | 58 |
| Admin-Portal | .230 | **Admin-Portal** - VM-Verwaltung | PRODUKTIV | 73 |
| Reverse-Proxy | .254 | **Proxy-Portal** - Nginx GUI | PRODUKTIV | 12 |
| DNS-Server | .216 | **DNS/DHCP** - Netzwerk-Infrastruktur | PRODUKTIV | 6 |
| DevoraXx | .214 | **DevoraXx** - Next.js + NestJS App | IN ENTWICKLUNG | 13 |
| edo | .246 | **edo** - Schulkommunikation | IN ENTWICKLUNG | 15 |
| Thea | .252 | **Thea** - Pflege-Assistent | IN ENTWICKLUNG | 8 |
| office | .253 | **Office** - Ticket-System + Flutter App | PRODUKTIV | 54 |
| cant | .166 | **CANT** - Chor-App (200 Mitglieder, 5 Standorte) | IN ENTWICKLUNG | 24 |
| PEDAGOGUS | .128 | **PEDAGOGUS** - Voting-Plattform für Bildung | IN ENTWICKLUNG | 4 |
| OpsRef | .150 | **OpsRef** - Aviation Reference Material | FRÜHE PHASE | - |
| OpenHAB | .10 | **Smart Home** - Hausautomation | PRODUKTIV | 14 |
| Stefan | .116 | **Stefans Terminal** - Lernumgebung | NEU | 1 |
| Marcel | .195 | **Marcels Portal** - Browser-Terminal | NEU | 3 |

---

## Details nach Kategorie

### PRODUKTIV (5 Systeme)

#### .15 SYSTEMHAUS-001 - EVY/MIRA
- **Was:** Multi-Tenant SaaS für KI-Assistenten
- **Features:** Chat-Widget, Brain-System, Voice, Telegram Live-Chat
- **Tech:** FastAPI, Vanilla JS, Claude/GPT APIs, ElevenLabs TTS
- **URLs:** systemhaus-horst.de/MIRA, mukupi.art

#### .230 Admin-Portal
- **Was:** Zentrales Verwaltungssystem für alle VMs
- **Features:** Dashboard, Terminal im Browser, Sessions, Snippets, VM-Management
- **Tech:** FastAPI + SvelteKit + PostgreSQL + Docker
- **73 Sessions dokumentiert!**

#### .254 Reverse-Proxy (Proxy-Portal)
- **Was:** Web-GUI für Nginx Reverse Proxy
- **Features:** Domain-Verwaltung, SSL-Zertifikate, Upstream-Config
- **Tech:** SvelteKit 5 + FastAPI + PostgreSQL + Docker
- **Verwaltet:** Alle externen Domains

#### .216 DNS-Server (FreeBSD)
- **Was:** Zentraler DNS/DHCP für ganzes Netzwerk
- **Tech:** Unbound DNS + ISC DHCP + Web-Portal
- **WICHTIG:** Fritzbox DHCP deaktiviert - dieser Server ist kritisch!

#### .253 office
- **Was:** Ticket-System + Office-Verwaltung
- **Features:** Flutter iOS App, Mail, Schreibstube
- **Tech:** SvelteKit + FastAPI
- **54 Sessions - sehr aktiv!**

### IN ENTWICKLUNG (5 Projekte)

#### .214 DevoraXx
- **Was:** Next.js + NestJS Anwendung
- **Letzte Arbeit:** CANT Analyse + Monetarisierung
- **Tech:** pnpm, Docker, PostgreSQL, Redis

#### .246 edo
- **Was:** Schulkommunikation (Vinzenz-von-Paul-Schule Beckum)
- **Status:** Email-Versand über Relay funktioniert
- **Code-Analyse:** Score B, 12 Findings
- **Nächste Schritte:** Security Fixes

#### .252 Thea
- **Was:** Pflege-Assistent für Alten-/Krankenpflege
- **Features:** Übergabe, Wunddokumentation
- **Tech:** SvelteKit 5 + FastAPI + SQLite
- **URL:** https://thea.systemhaus-horst.de

#### .166 cant (CANT - Choir App Network Tool)
- **Was:** Chor-Verwaltung für Stefan
- **Zielgruppe:** 200 Mitglieder an 5 Standorten
- **User:** Stefan (Chorleiter), Sabrina (Sängerin)
- **URL:** https://cant.systemhaus-horst.de

#### .128 PEDAGOGUS
- **Was:** Kollaboratives Prüfframework für Bildungsansätze
- **Features:** 424 Disziplinen, Voting-UI, Vorschläge
- **Status:** Voting-UI fertig

### FRÜHE PHASE / SPEZIAL (4 Systeme)

#### .150 OpsRef
- **Was:** Aviation Reference Material Platform
- **Status:** Changelog-basiert, frühe Phase

#### .10 OpenHAB
- **Was:** Smart Home Server
- **Tech:** OpenHAB Hausautomation
- **Status:** Läuft produktiv

#### .116 Stefan
- **Was:** Stefans Lern-Terminal
- **User:** Stefan (Dieters bester Freund, Claude-Anfänger)
- **Regel:** NUR lokal arbeiten!

#### .195 Marcel
- **Was:** Marcels Browser-Terminal
- **User:** Marcel (Dieters Sohn)
- **Feature:** Willkommens-Script mit Web-URL

---

## Physische Infrastruktur

| Gerät | IP | Funktion |
|-------|-----|----------|
| DASBIEST | .16 | Hyper-V Host, 128GB RAM, RTX 5080 |
| kleinerHund | .231 | Hyper-V Host |
| NASHORST | .126 | QNAP NAS, 2x 10TB ZFS, Backup |
| Mac Pro | .17 | Build-Server, 64GB RAM |
| Raspberry Pi | .11 | zigbee2mqtt |

---

## Zusammenfassung

**Gesamt: 14 VMs + 5 physische Geräte = 19 Maschinen**

**Nach Status:**
- 5 PRODUKTIV (MIRA, Admin-Portal, Proxy, DNS, Office)
- 5 IN ENTWICKLUNG (DevoraXx, edo, Thea, CANT, PEDAGOGUS)
- 4 SPEZIAL (OpsRef, OpenHAB, Stefan, Marcel)

**Sessions insgesamt:** ~260+ dokumentierte Sessions

**Das ist beeindruckend!** In wenigen Wochen ein komplettes Ökosystem aufgebaut.

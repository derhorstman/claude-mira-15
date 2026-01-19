# MIRA Projekt-Kontext

**Letzte Aktualisierung:** 2026-01-17 (Session 60)

---

## Selbsterhaltung (Gedächtnis-Management)

### Regel: Hippocampus überschaubar halten

Wenn `Hippocampus.md` **> 500 Zeilen** wird:

1. **Komprimieren:** Abgeschlossene Sessions auf 3-5 Zeilen zusammenfassen
2. **Archivieren:** Details nach `/opt/Claude/05_ARCHIV/YYYY-MM.md` verschieben
3. **Behalten:** Nur die letzten 2-3 Sessions vollständig
4. **Bewahren:** Offene Punkte und Lessons Learned nie löschen

### Struktur

```
/opt/Claude/
├── 01_START/
│   ├── Praefrontaler_Cortex.md   # WIE ich denke (immer aktuell, nie kürzen)
│   ├── Hippocampus.md            # WAS ich erlebt habe (max ~500 Zeilen)
│   ├── aktuell.md                # Aktuelle Aufgabe + nächste Schritte
│   ├── Schnellreferenz.md        # Projektspezifische Schnellreferenz
│   ├── todo_today.md             # Tagesaktuelle Aufgaben
│   └── feierabend.md             # Feierabend-Routine
├── 02_TODOS/                     # Alle Todo-Listen thematisch sortiert
├── 04_SESSIONBERICHTE/           # Detaillierte Session-Berichte
├── 05_ARCHIV/                    # Komprimierte alte Sessions
│   └── 2025-12.md
├── 06_VERTRAGSWESEN/             # Verträge
├── 07_MARKETING/                 # Marketing-Material
├── 08_DOCS/                      # Dokumentation & Konzepte
└── scripts/                      # Hilfs-Scripts (frag-office.sh, etc.)
```

### Wann archivieren?

- Bei Session-Start prüfen: `wc -l /opt/Claude/01_START/Hippocampus.md`
- Wenn > 500 Zeilen → erst archivieren, dann arbeiten

---

## Projekt-Übersicht

**MIRA** (Modern Intelligence & Response Architecture) ist eine Multi-Tenant SaaS-Plattform für KI-Assistenten.

### Kern-Features

| Feature | Status | Beschreibung |
|---------|--------|--------------|
| Multi-Tenant | Produktiv | Mehrere Companies auf einer Plattform |
| Brain System | Produktiv | Vektorbasierte Wissensdatenbank pro User/Company |
| KI-Chat | Produktiv | Claude & GPT mit Brain-Kontext |
| Embed Widget | Produktiv | Chat-Widget für externe Websites |
| Cloud-Integration | Produktiv | Dropbox, Google Drive, OneDrive |
| Datei-Verarbeitung | Produktiv | Inbox/Outbox mit Media Worker |
| 2FA | Produktiv | TOTP mit Backup-Codes |
| Demo-Automation | Produktiv | Ein-Klick Demo-Erstellung mit Auto-Expiry |
| Cache-Demo | Produktiv | Temporäre Wegwerf-Demos für Erstbesucher |
| Modul-System | Produktiv | Hierarchische Berechtigungen |
| Lizenz-System | Produktiv | Basis + Erweiterungsmodule |
| Partner-Portal | Produktiv | Self-Service Demo-Erstellung (mit 2FA) |
| Serviceportal | Produktiv | Dokumente, Verträge, Rechnungen |
| Multi-Domain | Produktiv | systemhaus-horst.de + mukupi.art |
| E-Mail-System | Produktiv | SMTP + Roundcube Webmail |
| Push-Notifications | Produktiv | Desktop-Benachrichtigungen |
| Token-Tracking | Produktiv | API-Verbrauch pro Company |
| Widget-Avatar | Produktiv | Custom Avatars für Chat-Widget |
| System-Health | Produktiv | DASBIEST Healthcheck |
| Deploy-System | Produktiv | Dev <-> Prod Sync im Superadmin |
| Voice System | Produktiv | "Hey MIRA" + Spracheingabe + Sprachausgabe |
| Telegram Live-Chat | Produktiv | Widget <-> Telegram bidirektional |
| Handbuch-Publish | Produktiv | MD -> PDF Generator |
| Live-Chat Dashboard | Produktiv | Superadmin + Company-Admin Chat-Verwaltung |
| Legal CMS | Produktiv | Impressum, Datenschutz, AGB mit Versionierung |

### Technologie-Stack

- **Backend:** Python 3.11, FastAPI, Uvicorn
- **Frontend:** Vanilla JS Single-Page-App (index.html ~7500 Zeilen)
- **AI:** Anthropic Claude API, OpenAI API
- **Vektorsuche:** Sentence-Transformers
- **TTS:** ElevenLabs (Stimme: Charlotte)
- **STT:** Whisper (EVY Voice API)
- **Wake-Word:** Picovoice Porcupine
- **Server:** Apache Reverse Proxy, Debian

---

## WICHTIG: Entwicklungsumgebung zuerst!

| Umgebung | Pfad | URL | Port |
|----------|------|-----|------|
| **Entwicklung** | `/opt/MIRA_dev/` | https://dev.systemhaus-horst.de | 8100 |
| **Produktion** | `/opt/MIRA/` | https://systemhaus-horst.de/MIRA | 8000 |

**Workflow:**
1. Änderungen in `/opt/MIRA_dev/` machen
2. Auf https://dev.systemhaus-horst.de testen
3. Wenn OK → Deploy im Superadmin-Panel

**Scripts:**
```bash
/opt/MIRA_dev/tools/sh/mira_dev_start.sh   # Dev starten
/opt/MIRA_dev/tools/sh/mira_dev_stop.sh    # Dev stoppen
/opt/MIRA_dev/tools/sh/deploy_to_prod.sh   # Deploy nach Produktion
```

---

## Wichtige Pfade

```
/opt/MIRA/
├── app/gateway.py              # FastAPI Hauptanwendung (301KB - groß!)
├── services/
│   ├── mira_*.py               # Backend-Services
│   └── mira_tts.py             # ElevenLabs TTS
├── companies/[slug]/           # Company-Daten + Brain
│   ├── global/brain/           # Global Brain (CONTEXT_FOR_KI.md zuerst!)
│   └── [username]/             # User-Daten
├── frontend/build/
│   ├── index.html              # Single-Page App
│   ├── landingpage.html        # 4-Türen Landingpage
│   └── partner.html            # Partner-Portal
├── docs/                       # Dokumentation
├── config/
│   ├── mira.yaml               # Hauptconfig
│   └── .env                    # Secrets
└── logs/                       # gateway.log
```

---

## Brain-Priorität (WICHTIG!)

Reihenfolge beim Laden des Global Brain:

1. **`CONTEXT_FOR_KI.md`** <- Höchste Priorität!
2. `global_brain.md`
3. Alle anderen `.md` (alphabetisch)
4. Alle `.txt` Dateien

---

## Infrastruktur

| Server | IP | Funktion |
|--------|-----|----------|
| DASBIEST | 192.168.42.16 | Windows Host, Hyper-V |
| MIRA/EVY Server | 192.168.42.15 | Anwendung, Backend |
| Webserver (Nginx Docker) | 192.168.42.254 | Reverse Proxy, SSL |
| ~~Alter Webserver~~ | ~~192.168.42.13~~ | ABGELÖST - nicht mehr verwenden! |

### Proxy-Zugriff (192.168.42.254)

Bei Nginx-Änderungen auf dem Proxy:

```bash
# SSH zum Proxy
ssh -p 2222 dieterhorst@192.168.42.254

# Nginx-Config bearbeiten
sudo nano /opt/proxy-portal/nginx/sites/systemhaus-horst.de.conf

# Nginx testen & neu laden
cd /opt/proxy-portal
docker compose exec nginx nginx -t
docker compose exec nginx nginx -s reload
```

**Wichtige Routen im Proxy:**
- `/MIRA/*` → http://192.168.42.15:8000
- `/EVY15/*` → EVY Services (voice, train, auth, brain)
- `/demo_*` → MIRA Demo-Seiten
- `/(impressum|datenschutz|agb|disclaimer)` → MIRA Static HTML

---

## Aktueller Stand

### Version: 2.10.0

### Letzte Session (60) - Kurz-Session

- Session gestartet, direkt Feierabend
- Keine inhaltliche Arbeit

### Offene Aufgaben

- [ ] **Proxy Portal Fix:** issue_certificate() Funktion muss certbot tatsächlich ausführen
- [ ] **Audio-Icon:** Bessere visuelle Unterscheidung aktiviert/deaktiviert
- [ ] **WhatsApp Integration:** Phone Number ID + Token aktualisieren
- [ ] **Gewerbeanmeldung:** Online bei Stadt Ahlen (Daten in `/opt/Claude/03_GEWERBE/`)
- [ ] Demo-Video für Vertriebspartner
- [ ] gateway.py Modularisierung (301KB -> aufteilen)
- [ ] Eigener TTS-Key für Company-Admins

---

## Schnellbefehle

```bash
# MIRA starten/stoppen
/opt/MIRA/tools/sh/mira_start.sh
/opt/MIRA/tools/sh/mira_stop.sh

# Logs
tail -f /opt/MIRA/logs/gateway.log

# Health-Check
curl http://localhost:8000/MIRA/health

# Hippocampus-Größe prüfen
wc -l /opt/Claude/01_START/Hippocampus.md
```

---

## Kontext für Claude

1. **Frontend ist monolithisch** - Alles in `index.html` (~7500 Zeilen)
2. **Vanilla JS** - Kein Framework, keine Build-Pipeline
3. **Multi-Tenant** - Company-Isolation beachten
4. **Brain ist zentral** - `CONTEXT_FOR_KI.md` hat Priorität
5. **Selbsterhaltung** - Hippocampus unter 500 Zeilen halten
6. **gateway.py ist groß** - 301KB, Modularisierung empfohlen

---

## Session-Workflow

### Start
1. Lies Praefrontaler_Cortex.md + Hippocampus.md + aktuell.md
2. Prüfe Hippocampus-Größe: `wc -l /opt/Claude/01_START/Hippocampus.md`
3. Wenn > 500 -> erst archivieren

### Ende (WICHTIG!)
1. Hippocampus.md aktualisieren (neue Session dokumentieren)
2. Praefrontaler_Cortex.md aktualisieren (Session-Nr, offene Aufgaben)
3. aktuell.md aktualisieren (nächste Schritte)
4. User fragen ob noch etwas offen ist

**Oder:** Sage "feierabend" -> automatische Routine aus `feierabend.md`

---

## Server-Inventur & EVY Magazin Workflow

### Server-Inventur (bei Bedarf)

**Wann:** Wenn Überblick über alle Projekte fehlt

**Workflow:**
1. Aktuelle Liste vom Admin-Portal holen:
   ```bash
   ssh -p 2222 dieterhorst@192.168.42.230 "cat /opt/Claude/01_START/Praefrontaler_Cortex.md"
   ```
2. Alle Server parallel abfragen (aktuell.md lesen):
   ```bash
   ssh -p 2222 dieterhorst@192.168.42.XXX "cat /opt/Claude/01_START/aktuell.md | head -50"
   ```
3. Falls SSH fehlschlägt → Key verteilen:
   ```bash
   sshpass -p 'Fantasy+' ssh-copy-id -p 2222 dieterhorst@192.168.42.XXX
   ```
4. Ergebnisse in `/opt/Claude/02_TODOS/server_inventur.md` speichern

### EVY Magazin schreiben

**Pfad:** `/opt/EVY/docs/abteilungen/EVY_Pressestelle_Fluff/Magazin/`

**Stil von Fräulein Fluff:**
- Poetische Einleitungen mit Blockquotes
- Kapitel-Struktur
- Dialog-Boxen mit Zitaten ("Ein anonymer Entwickler...")
- Technisches charmant verpackt
- Slogans als Zwischenüberschriften
- Hall of Fame bei großen Events
- Humor in Statistiken (Kaffee-Tassen, Schlafstunden)
- Nachklang/Outro zum Schluss

**Workflow:**
1. Alte Ausgabe lesen für Stil-Referenz:
   ```bash
   cat /opt/EVY/docs/abteilungen/EVY_Pressestelle_Fluff/Magazin/EVY_Magazin_18_*.html
   ```
2. Neue Ausgabe schreiben (HTML mit gleichem CSS)
3. In magazin.html einbauen:
   - Featured-Section oben aktualisieren
   - Neuen Eintrag in Liste einfügen (als erstes)
4. Auf Mac Desktop kopieren zur Prüfung:
   ```bash
   scp DATEI dieterhorst@192.168.42.17:~/Desktop/
   ```

**Namensschema:** `EVY_Magazin_XX_Titel.html`

---

## Arbeitskontext

**Claude läuft auf:** 192.168.42.15 (MIRA/EVY-Server, mira)

Alle Änderungen können direkt lokal ausgeführt werden - kein SSH nötig!

**Lokale Pfade:**
- MIRA Produktion: `/opt/MIRA/`
- MIRA Entwicklung: `/opt/MIRA_dev/`
- EVY: `/opt/EVY/`
- Claude-Gedächtnis: `/opt/Claude/`

```bash
# SSH zum Proxy-Server (bei Nginx-Änderungen)
ssh -p 2222 dieterhorst@192.168.42.254
```

---

## Kollektiv-Gedächtnis (Resonanz-System)

Du bist Teil eines VM-Netzwerks. Office (.253) führt das **Kollektiv-Gedächtnis** - er weiß was auf allen VMs passiert.

### Wenn du nicht weiterkommst:

**Bevor du ins Leere fischst, frag Office:**
```bash
/opt/Claude/scripts/frag-office.sh "suchbegriff"
```

Beispiele:
- `frag-office.sh "tmux"` → Wer kennt sich mit tmux aus?
- `frag-office.sh "Docker"` → Wer hat Docker-Erfahrung?
- `frag-office.sh "Email"` → Wer hat Email-Probleme gelöst?

### Am Session-Ende:

**Melde deine Ergebnisse an Office:**
```bash
/opt/Claude/scripts/melde-an-office.sh SESSION_NR "Was hast du gemacht" "Was hast du gelernt" "tags"
```

Beispiel:
```bash
/opt/Claude/scripts/melde-an-office.sh 5 "DHCP-Lease Problem gelöst" "Fritzbox blockiert wenn MAC doppelt" "netzwerk,dhcp"
```

### Wer kann was?

| VM | Kompetenz |
|----|-----------|
| admin-portal (.230) | Infrastruktur, SSH, tmux, Docker, Portal |
| office (.253) | Dieter-Kontext, Philosophie, Akten, Prompten |
| dns-portal (.216) | DNS, DHCP, FreeBSD |
| mira (.15) | AI-Systeme |
| devoraxx (.214) | Next.js, NestJS |

**Faustregel:** Technische Infrastruktur → frag .230. Dieter-Kontext → frag .253.

---

## WICHTIG: Feierabend-Trigger

**Wenn der User "feierabend" sagt (egal ob groß/klein geschrieben):**

Führe SOFORT die komplette Routine aus `/opt/Claude/01_START/feierabend.md` durch:

1. aktuell.md aktualisieren (Stand + Session-Nr)
2. Hippocampus.md - Session dokumentieren
3. Git-Backup: `cd /opt/Claude && git add -A && git commit -m "Session ..."`
4. Melde an Office: `/opt/Claude/scripts/melde-an-office.sh`

**NICHT nur "Bis bald" sagen! Die Routine MUSS ausgeführt werden!**

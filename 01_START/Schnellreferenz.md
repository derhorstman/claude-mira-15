# Schnellreferenz - mira (192.168.42.15)

**Letzte Aktualisierung:** 2026-01-14 (Session 59)

---

## 1. Projektspezifisches

### Was läuft hier?

**MIRA** (Modern Intelligence & Response Architecture) - Multi-Tenant SaaS-Plattform für KI-Assistenten

**EVY** - Voice System mit Wake-Word "Hey MIRA"

### Services & Ports

| Service | Port | URL | Beschreibung |
|---------|------|-----|--------------|
| MIRA Produktion | 8000 | https://systemhaus-horst.de/MIRA | Haupt-Anwendung |
| MIRA Entwicklung | 8100 | https://dev.systemhaus-horst.de | Test-Umgebung |
| EVY Voice | 8002 | intern | Sprach-Services |
| Apache | 80/443 | - | Reverse Proxy lokal |

### Wichtige Pfade

```
/opt/MIRA/              # Produktion
/opt/MIRA_dev/          # Entwicklung (HIER ZUERST ÄNDERN!)
/opt/EVY/               # Voice System
/opt/Claude/            # Mein Gedächtnis
```

### Technologie-Stack

- Python 3.11, FastAPI, Uvicorn
- Vanilla JS Frontend (kein Framework!)
- Claude API + OpenAI API
- ElevenLabs TTS, Whisper STT
- Picovoice Porcupine Wake-Word

---

## 2. Häufige Befehle

### MIRA starten/stoppen

```bash
# Produktion
/opt/MIRA/tools/sh/mira_start.sh
/opt/MIRA/tools/sh/mira_stop.sh

# Entwicklung
/opt/MIRA_dev/tools/sh/mira_dev_start.sh
/opt/MIRA_dev/tools/sh/mira_dev_stop.sh

# Deploy von Dev nach Prod
/opt/MIRA_dev/tools/sh/deploy_to_prod.sh
```

### Worker

```bash
/opt/MIRA/tools/sh/mira_worker_start.sh      # Outbox Worker
/opt/MIRA/tools/sh/mira_worker_stop.sh
/opt/MIRA/tools/sh/mira_media_worker_start.sh
/opt/MIRA/tools/sh/mira_media_worker_stop.sh
```

### Logs & Health

```bash
# Logs live
tail -f /opt/MIRA/logs/gateway.log

# Health-Check
curl http://localhost:8000/MIRA/health

# Hippocampus-Größe (< 500 Zeilen halten!)
wc -l /opt/Claude/01_START/Hippocampus.md
```

### Handbuch publizieren

```bash
/opt/MIRA/tools/sh/publish_manuals.sh
```

---

## 3. Fehler und Lösungen

### EVY Login hängt bei "Prüfe Zugang..."

**Problem:** Firewall blockiert Port 8002 vom Proxy
**Lösung:**
```bash
sudo ufw allow from 192.168.42.254 to any port 8002
```

### SSH zu anderen Servern fehlgeschlagen

**Problem:** SSH-Key nicht verteilt
**Lösung:**
```bash
sshpass -p 'Fantasy+' ssh-copy-id -p 2222 dieterhorst@192.168.42.XXX
```

### Nginx-Änderungen greifen nicht

**Problem:** Config auf falschem Server geändert
**Lösung:** Proxy läuft auf .254, NICHT auf .13 (alter Webserver abgelöst!)
```bash
ssh -p 2222 dieterhorst@192.168.42.254
cd /opt/proxy-portal
docker compose exec nginx nginx -t
docker compose exec nginx nginx -s reload
```

### Brain-Priorität beachten

**Problem:** KI ignoriert wichtige Infos
**Lösung:** `CONTEXT_FOR_KI.md` wird zuerst geladen - wichtige Infos dort rein!

---

## 4. Credentials & Config

### Config-Dateien (NUR PFADE!)

| Was | Pfad |
|-----|------|
| MIRA Hauptconfig | `/opt/MIRA/config/mira.yaml` |
| Secrets (.env) | `/opt/MIRA/config/.env` |
| Company-Configs | `/opt/MIRA/companies/[slug]/` |
| Gewerbedaten | `/opt/Claude/03_GEWERBE/` |

### API-Keys liegen in

- `/opt/MIRA/config/.env` (Anthropic, OpenAI, ElevenLabs)
- Company-spezifische Keys in `/opt/MIRA/companies/[slug]/config/`

---

## 5. Kontakte zu anderen Servern

### Netzwerk-Übersicht

| Server | IP | SSH Port | Funktion |
|--------|-----|----------|----------|
| **Reverse-Proxy** | .254 | 2222 | Nginx Docker, SSL, zentral |
| Admin-Portal | .230 | 2222 | Zentrales Verwaltungssystem |
| DevoraXx | .214 | 2222 | Next.js + NestJS |
| DNS-Server | .216 | 2222 | FreeBSD, Unbound + DHCP |
| office | .253 | 22 | Ticket-System |
| Thea | .252 | 2222 | Pflege-Assistent |
| edo | .246 | 2222 | Schulkommunikation |
| OpenHAB | .10 | 2222 | Smart Home |
| Nextcloud | .12 | 2222 | Cloud + Home Assistant |
| Mac (Chef) | .17 | 22 | Build-Server, Desktop |

### SSH-Befehle

```bash
# Zum Proxy (bei Nginx-Änderungen)
ssh -p 2222 dieterhorst@192.168.42.254

# Zum Admin-Portal (Server-Übersicht)
ssh -p 2222 dieterhorst@192.168.42.230

# Datei auf Mac Desktop kopieren
scp DATEI dieterhorst@192.168.42.17:~/Desktop/
```

### Hosts (physisch)

| Name | IP | Beschreibung |
|------|-----|--------------|
| DASBIEST | .16 | Hyper-V Host, 128GB RAM, RTX 5080 |
| kleinerHund | .231 | Hyper-V Host |
| NASHORST | .126 | QNAP NAS, 2x 10TB ZFS |
| Raspberry Pi | .11 | zigbee2mqtt |

---

## Selbst-Erinnerungen

- **IMMER zuerst in /opt/MIRA_dev/ ändern, dann deployen!**
- **Hippocampus.md unter 500 Zeilen halten**
- **gateway.py ist 301KB groß - vorsichtig bearbeiten**
- **Frontend ist monolithisch (index.html ~7500 Zeilen)**
- **Alter Webserver .13 ist ABGELÖST - nicht mehr verwenden!**

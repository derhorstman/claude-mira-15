# Hippocampus - Session-Gedächtnis

**Server:** mira (192.168.42.15)

Chronologische Dokumentation der Entwicklungsarbeit.

**Archiviert:** Sessions 1-49 → `/opt/Claude/05_ARCHIV/2025-12.md`

---

## WICHTIG: Nach jeder Session

**Am Ende jeder Session diese Dateien aktualisieren:**

1. **Hippocampus.md** - Session-Zusammenfassung hinzufügen
2. **Praefrontaler_Cortex.md** - Session-Nr, Version, offene Aufgaben
3. **aktuell.md** - Aktuellen Stand + nächste Schritte

**Hippocampus-Limit:** Max 500 Zeilen! Bei Überschreitung archivieren.

---

## Template für neue Einträge

```markdown
## YYYY-MM-DD: Session XX - [Titel]

### Durchgeführte Arbeiten
- [Punkt 1]
- [Punkt 2]

### Geänderte Dateien
| Datei | Änderung |
|-------|----------|

### Offene Punkte
- [ ] [Punkt]
```

---

## Notizen

### Bekannte Eigenheiten

1. **index.html ist sehr groß** (~7500 Zeilen)
2. **Keine Build-Pipeline** - Änderungen direkt in build/
3. **modules.js inline** wegen Apache Proxy
4. **State ist global** - currentUser, authToken, etc.
5. **gateway.py ist 301KB** - Modularisierung empfohlen

### Brain-Priorität

Reihenfolge beim Laden des Global Brain:
1. `CONTEXT_FOR_KI.md` (höchste Priorität)
2. `global_brain.md`
3. Alle anderen `.md` (alphabetisch)
4. Alle `.txt` Dateien

### Nützliche Befehle

```bash
# Hippocampus-Größe prüfen (max 500 Zeilen)
wc -l /opt/Claude/01_START/Hippocampus.md

# MIRA neu starten
/opt/MIRA/tools/sh/mira_stop.sh && /opt/MIRA/tools/sh/mira_start.sh

# Dev starten/stoppen
/opt/MIRA_dev/tools/sh/mira_dev_start.sh
/opt/MIRA_dev/tools/sh/mira_dev_stop.sh

# Logs
tail -f /opt/MIRA/logs/gateway.log

# SSH zum Webserver (NEUER Proxy!)
ssh -p 2222 dieterhorst@192.168.42.254
# ACHTUNG: 192.168.42.13 ist abgelöst - nicht mehr verwenden!
```

---

## Archivierte Sessions (Kurzfassung)

| Sessions | Datum | Highlights |
|----------|-------|------------|
| 1-8 | 01.12. | Modul-System, Demo Auto-Expiry, Lizenz-System |
| 9-11 | 01.12. | Partner-Portal, Web-Scraper, Sales-Page |
| 12-14 | 02.12. | Serviceportal, Multi-Domain, Slogan |
| 15-17 | 02-03.12. | E-Mail, Push, Brain-Inhalte |
| 18-22 | 03-04.12. | VM-Migration, Cache-Demo, Rate Limiting, BIEST |
| 23-25 | 05.12. | Filter, Dev-Umgebung, Healthcheck |
| 26-28 | 06.12. | Deploy-System, Token-Tracking, Widget-Avatar |
| 29-30 | 06.12. | Voice System (TTS, STT, Wake-Word) |
| 31-34 | 07.12. | MiraCoin, Handbücher, Jascha-Demo |
| 35-37 | 07.12. | Telegram Live-Chat, Live-Chat Dashboard |
| 38-40 | 08.12. | Gedächtnis-Setup, Demo-Video Planung, Legal CMS |
| 41-49 | 09-14.12. | Proxy-Migration, WhatsApp 90%, Inka Oldenburger |

Details: `/opt/Claude/05_ARCHIV/2025-12.md`

---

## Aktuelle Sessions

## 2025-12-14: Session 50 - Self-Preservation Migration + SSL Fix

### Durchgeführte Arbeiten
- Claude-Gedächtnis von einzelnen Servern nach .230 zentralisiert
- SSL-Zertifikat dev.systemhaus-horst.de manuell ausgestellt
- Upstream mira_dev (Port 8100) konfiguriert
- Dev/Prod synchronisiert

---

## 2025-12-14: Session 51 - Disaster Recovery

### Durchgeführte Arbeiten
- Selbsterhaltungs-Ordner (02_TODOS, 05_ARCHIV, 06_VERTRAGSWESEN) fehlten
- Ursache: Session 20 auf .230 hatte Struktur "reorganisiert"
- Recovery via VHDX-Backup (14.12. 03:00)
- Alle Daten wiederhergestellt

### Lesson Learned
- **Backups funktionieren!** Nacht-Backup hat den Tag gerettet

---

## 2025-12-15: Session 52 - MiraCoin Superadmin UI + Provisionsvertrag

### Durchgeführte Arbeiten
- MiraCoin System komplett (500 MC für sales.io)
- Superadmin MiraCoins-Interface (Wallets, Bonus, Abzug, Auszahlung)
- MiraCoin-Provisionshandbuch erstellt
- Provisionsvertrag für Inka Oldenburger
- Stammdaten `/opt/Claude/00_STAMMDATEN/dieter_horst.md`
- Verkaufsflyer erstellt

---

## 2025-12-17: Session 53 - Thea TTS-Fix

### Durchgeführte Arbeiten
- Company "Thea" konnte Widget nicht sprechen lassen
- Ursache: `tts_provider: openai` aber kein OpenAI API-Key
- Lösung: OpenAI-Key in companies.json hinzugefügt

---

## 2025-12-18: Session 54 - Selbsterhaltung Putztag

### Durchgeführte Arbeiten
- Hippocampus komprimiert (510 → ~140 Zeilen)
- Sessions 41-49 ins Archiv verschoben
- Pfade korrigiert (MIRA/ Unterordner entfernt)
- Doppelte Trennlinien bereinigt

---

## 2025-12-20: Session 55 - Selbsterhaltung Putztag II

### Durchgeführte Arbeiten
- feierabend.md bereinigt (Alexa-Zeug entfernt, MIRA-spezifisch)
- Praefrontaler_Cortex.md Pfad korrigiert
- "Zeig mal" mit Hostname-Konvention in aktuell.md ergänzt
- tipp_vom_dns-portal.md gelöscht (Nützliches übernommen)

---

## 2025-12-21: Session 56 - VVP Widget-Debugging

### Durchgeführte Arbeiten
- VVP Company beklagte Widget funktioniert nicht
- MIRA Health-Check: läuft (degraded - claude/voice services down)
- VVP Embed-Key ermittelt: `emb_615a8b0076b449ee541f93e9`
- Widget-Endpunkt funktioniert: `/api/embed/widget.js?key=...`
- Problem: edo (246) löst systemhaus-horst.de auf externe IP (217.86.150.215) → Timeout
- Lösung: `/etc/hosts` Eintrag auf edo: `192.168.42.254 systemhaus-horst.de`
- mira.db ist 0 Bytes - aber irrelevant (MIRA nutzt JSON-Dateien)

### Erkenntnisse
- MIRA speichert alles in JSON (companies.json, etc.) - keine SQLite
- Interne Server brauchen hosts-Einträge für lokale Domain-Auflösung

---

## 2026-01-02: Session 57 - Konsistenzprüfung Frontend/Backend

### Durchgeführte Arbeiten
- Vollständige Konsistenzprüfung MIRA durchgeführt
- gateway.py (9469 Zeilen) vs index.html (15378 Zeilen) analysiert
- ~200 Backend-Endpoints, ~120 Frontend API-Calls verglichen

### Kritische Funde
- **WhatsApp-Backend fehlt komplett!** Frontend hat UI, aber 4 Endpoints fehlen:
  - `/company/whatsapp/settings` (GET/PUT)
  - `/company/whatsapp/test` (POST)
  - `/superadmin/company/{slug}/whatsapp` (POST)
- `/portal/uploads` - Frontend ruft auf, Backend fehlt

### Report
- `konsistenz-report-MIRA.md` auf Mac Desktop abgelegt
- Auth, Schemas, Error-Handling: OK
- Fehlermeldungen gemischt DE/EN (Minor)

---

## 2026-01-06: Session 58 - Server-Inventur & EVY Magazin

### Durchgeführte Arbeiten
- EVY Login Fix (Firewall Port 8002 für neuen Proxy .254)
- SSH-Keys auf alle 8 fehlenden Server verteilt
- Große Server-Inventur: 19 Maschinen, 260+ Sessions dokumentiert
- EVY Magazin Ausgabe 19 "Das Imperium" geschrieben
- Workflow für Inventur & Magazin in Praefrontaler_Cortex.md eingebaut

### Geänderte Dateien
| Datei | Änderung |
|-------|----------|
| /opt/Claude/02_TODOS/server_inventur.md | Neu erstellt |
| /opt/EVY/docs/.../EVY_Magazin_19_*.html | Magazin geschrieben |

---

## 2026-01-14: Session 59 - Selbsterhaltung & Hostname

### Durchgeführte Arbeiten
- Hostname von SYSTEMHAUS-001 auf "mira" geändert
- Schnellreferenz.md komplett gefüllt (192 Zeilen)
- Selbsterhaltungs-Dateien auf Konsistenz geprüft

---

## Offene Punkte

- [ ] Proxy Portal: issue_certificate() Backend-Fix
- [ ] Eigener TTS-Key für Company-Admins
- [ ] Audio-Icon Unterscheidung
- [ ] WhatsApp Integration (Phone Number ID + Token)
- [ ] Gewerbeanmeldung
- [ ] Demo-Video
- [ ] gateway.py Modularisierung

---

**Zuletzt aktualisiert:** 2026-01-14 (Session 59)

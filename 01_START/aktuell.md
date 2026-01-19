# Aktuelle Aufgabe

**Stand:** 2026-01-19 (Session 61)

---

## JETZT

**Status:** Bereit für nächste Aufgabe
**Letzte Arbeit:** Session 61 - mukupi.art Seiten wieder online

---

## Was wurde erledigt (Session 58)

### EVY Login Fix
- [x] Problem: Login hing bei "Prüfe Zugang..."
- [x] Ursache: Firewall Port 8002 nur für alten Webserver (.13) offen
- [x] Fix: `sudo ufw allow from 192.168.42.254 to any port 8002`
- [x] EVY Login funktioniert wieder

### Server-Inventur
- [x] SSH-Keys auf alle 8 fehlenden Server verteilt
- [x] Alle 14 VMs + 5 physische Geräte inventarisiert
- [x] 260+ Sessions dokumentiert
- [x] Inventur gespeichert: `/opt/Claude/02_TODOS/server_inventur.md`

### EVY Magazin Ausgabe 19
- [x] "Das Imperium - Eine Bestandsaufnahme" geschrieben
- [x] Fräulein Fluff Stil beibehalten
- [x] In magazin.html als aktuelle Ausgabe eingebaut
- [x] Pfad: `/opt/EVY/docs/abteilungen/EVY_Pressestelle_Fluff/Magazin/EVY_Magazin_19_Das_Imperium.html`

### Selbsterhaltung
- [x] Workflow für Inventur & Magazin in Praefrontaler_Cortex.md eingebaut

---

## GROSSE INVENTUR - Server & Projekte

**Ziel:** Alle Server durchgehen, Projekte dokumentieren, Material für EVY Magazin sammeln

### Hosts (physisch)

| Name | IP | Status | Notizen |
|------|-----|--------|---------|
| DASBIEST | .16 | [ ] | Hyper-V Host, 128GB RAM, RTX 5080 |
| kleinerHund | .231 | [ ] | Hyper-V Host |
| NASHORST | .126 | [ ] | QNAP NAS, 2x 10TB ZFS |
| Mac Pro | .17 | [ ] | 64GB RAM, Build-Server |
| Raspberry Pi | .11 | [ ] | zigbee2mqtt |

### VMs auf DASBIEST

| Name | IP | Status | Projekt |
|------|-----|--------|---------|
| Admin-Portal | .230 | [x] | Zentrales Verwaltungssystem |
| SYSTEMHAUS-001 | .15 | [x] | EVY/MIRA - bekannt |
| DevoraXx | .214 | [ ] | Next.js + NestJS |
| DNS-Server | .216 | [ ] | FreeBSD, Unbound + DHCP |
| Reverse-Proxy | .254 | [ ] | Nginx Docker, zentral |
| office | .253 | [ ] | Ticket-System, Flutter App |
| Thea | .252 | [ ] | Pflege-Assistent |
| edo | .246 | [ ] | Schulkommunikation |
| PEDAGOGUS | .128 | [ ] | Voting-Plattform |
| OpsRef | .150 | [ ] | Aviation Reference |
| cant | .166 | [ ] | Chor-Software (Stefans) |
| cant-dev | .174 | [ ] | Entwicklung |
| Stefan | .116 | [ ] | Stefans Portal |
| Marcel | .195 | [ ] | Marcels Terminal-Portal |

### VMs auf kleinerHund

| Name | IP | Status | Projekt |
|------|-----|--------|---------|
| Webserver | .13 | [ ] | Apache2, 9 Domains (ALTBESTAND) |
| OpenHAB | .10 | [ ] | Smart Home |
| Nextcloud | .12 | [ ] | Cloud + Home Assistant |

**Workflow:** Chef zeigt Server -> Claude schaut rein -> Dokumentation -> Magazin-Material

---

## Was wurde erledigt (Session 57)

### Konsistenzprüfung Frontend/Backend
- [x] gateway.py (9469 Zeilen) analysiert
- [x] index.html (15378 Zeilen) analysiert
- [x] ~200 Backend-Endpoints vs ~120 Frontend-Calls verglichen
- [x] Kritischer Fund: WhatsApp-Backend fehlt komplett (4 Endpoints)
- [x] Minor: `/portal/uploads` fehlt im Backend
- [x] Report auf Mac Desktop: `konsistenz-report-MIRA.md`

---

## Offene Punkte

- [ ] **Proxy Portal Fix:** issue_certificate() Funktion muss certbot tatsächlich ausführen
- [ ] **Eigener TTS-Key:** Company-Admins ElevenLabs-Key im Frontend
- [ ] **Audio-Icon:** Bessere visuelle Unterscheidung aktiviert/deaktiviert
- [ ] **WhatsApp Integration:** Phone Number ID + Token
- [ ] **Gewerbeanmeldung:** Online bei Stadt Ahlen
- [ ] **Demo-Video**
- [ ] gateway.py Modularisierung

---

## Schnellstart

```bash
# MIRA Status
curl http://localhost:8000/MIRA/health

# Logs
tail -f /opt/MIRA/logs/gateway.log

# Hippocampus prüfen
wc -l /opt/Claude/01_START/Hippocampus.md
```

---

## Selbsterhaltung

| Datei | Max Zeilen | Aktuell |
|-------|------------|---------|
| Hippocampus.md | 500 | 254 |

**Regel:** Bei > 500 Zeilen -> archivieren!

---

## Zeig mal (Mac Desktop)

**Bei "zeig mal X" → Datei/Ordner auf Mac Desktop kopieren**

```bash
# Einzelne Datei
scp /pfad/zur/datei.md dieterhorst@192.168.42.17:~/Desktop/

# Ordner (mit Hostname im Namen)
scp -r /opt/Claude dieterhorst@192.168.42.17:~/Desktop/Claude_SYSTEMHAUS-001
```

Der Chef kann dann die Datei direkt auf seinem Mac sehen und bearbeiten.

---

## Netzwerk-Infrastruktur (Stand: 2025-12-20)

### Alle Server - Statische IPs

| Name | IP | SSH Port | Funktion |
|------|-----|----------|----------|
| office | .253 | 22 | Office-Server |
| OpenHAB | .10 | 2222 | Smart Home |
| zigbee2mqtt | .11 | 2222 | Zigbee-MQTT Bridge (WLAN) |
| Nextcloud | .12 | 2222 | Cloud + Home Assistant |
| SYSTEMHAUS-001 | .15 | 2222 | EVY/MIRA AI-System |
| DevoraXx | .214 | 2222 | Next.js + NestJS Projekt |
| DNS-Server | .216 | 2222 | Unbound DNS + ISC DHCP (FreeBSD) |
| Admin-Portal | .230 | 2222 | Zentrales Admin-Portal |
| edo | .246 | 2222 | Schulkommunikation |
| Thea | .252 | 2222 | Pflegedokumentation |
| Reverse-Proxy | .254 | 2222 | Nginx Reverse Proxy |

### Netzwerk
- **Gateway:** 192.168.42.1 (Fritzbox)
- **DNS/DHCP:** 192.168.42.216
- **Mac (Chef):** 192.168.42.17

**Alle VMs haben statische IPs!**

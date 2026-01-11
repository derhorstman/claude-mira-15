# Dev-VM aufsetzen

**Priorität:** Hoch
**Aufwand:** 1-2 Stunden

## Ziel
Separate Entwicklungs-VM damit Produktion (4 Kunden!) unberührt bleibt.

## Setup
- Neue VM auf DASBIEST: Debian 12, 4GB RAM, 50GB Disk
- IP: 192.168.42.XX (frei wählen)
- MIRA rüberkopieren (rsync)
- Claude-Verzeichnis /opt/Claude/ rüber
- Nginx-Route: dev.systemhaus-horst.de

## Danach
- 192.168.42.15 = Produktion (heilig, nur Deploy)
- 192.168.42.XX = Entwicklung + Claude-Sessions

## Bonus
- Guter Testlauf für späteren Installer
- Jeden Schritt dokumentieren

---
Erstellt: 2025-12-13 (Session 47)

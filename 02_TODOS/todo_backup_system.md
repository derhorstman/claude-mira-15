# Todo: Backup-System bei Dev→Prod Sync

**Status:** ✅ Erledigt

**Ziel:** Vor jedem Deploy von Dev nach Prod automatisch ein Backup erstellen, das einfach zurückgespielt werden kann.

---

## Problem

Aktuell bei `/opt/MIRA_dev/tools/sh/deploy_to_prod.sh`:
- Dateien werden überschrieben
- Kein Backup
- Bei Fehlern: Manuelles Wiederherstellen nötig

---

## Anforderungen

### Automatisches Backup

- [x] Vor Deploy: Backup von Prod erstellen
- [x] Backup-Ordner mit Timestamp: `/opt/MIRA/backups/2025-12-07_08-30-00/`
- [x] Was sichern:
  - `app/` (komplett)
  - `frontend/build/` (komplett)
  - `services/` (komplett)

### Rollback-Funktion

- [x] Script: `mira_rollback.sh`
- [x] Zeigt verfügbare Backups mit Nummerierung
- [x] Wählt Backup aus → Spielt zurück
- [x] Startet MIRA neu

### Cleanup

- [x] Alte Backups automatisch löschen (behalte max. 10) in deploy_to_prod.sh
- [x] Manuell: `mira_backup_cleanup.sh`

---

## Technische Umsetzung

### Backup-Struktur

```
/opt/MIRA/backups/
├── 2025-12-07_08-30-00/
│   ├── app/
│   │   └── gateway.py
│   ├── frontend/
│   │   └── build/
│   │       └── index.html
│   ├── services/
│   │   └── *.py
│   └── manifest.json  # Was wurde gesichert, von wem, warum
└── 2025-12-06_14-20-00/
    └── ...
```

### manifest.json

```json
{
  "timestamp": "2025-12-07T08:30:00",
  "created_by": "deploy_to_prod.sh",
  "reason": "Pre-deploy backup",
  "files": [
    "app/gateway.py",
    "frontend/build/index.html",
    "services/mira_coins.py"
  ],
  "prod_version": "2.9.0",
  "dev_version": "2.9.1"
}
```

### deploy_to_prod.sh Änderungen

```bash
#!/bin/bash
# Vor dem Kopieren:
BACKUP_DIR="/opt/MIRA/backups/$(date +%Y-%m-%d_%H-%M-%S)"
mkdir -p "$BACKUP_DIR"

# Backup erstellen
cp -r /opt/MIRA/app "$BACKUP_DIR/"
cp -r /opt/MIRA/frontend/build "$BACKUP_DIR/frontend/"
cp -r /opt/MIRA/services "$BACKUP_DIR/"

echo "Backup erstellt: $BACKUP_DIR"

# Dann Deploy wie bisher...
```

### mira_rollback.sh

```bash
#!/bin/bash
echo "Verfügbare Backups:"
ls -1 /opt/MIRA/backups/ | tail -10

read -p "Backup wählen: " BACKUP_NAME
BACKUP_DIR="/opt/MIRA/backups/$BACKUP_NAME"

if [ -d "$BACKUP_DIR" ]; then
    /opt/MIRA/tools/sh/mira_stop.sh
    cp -r "$BACKUP_DIR/app/"* /opt/MIRA/app/
    cp -r "$BACKUP_DIR/frontend/"* /opt/MIRA/frontend/
    cp -r "$BACKUP_DIR/services/"* /opt/MIRA/services/
    /opt/MIRA/tools/sh/mira_start.sh
    echo "Rollback abgeschlossen!"
else
    echo "Backup nicht gefunden!"
fi
```

---

## Integration im Frontend

### Superadmin Deploy-Page

- [ ] Letzte Backups anzeigen
- [ ] Rollback-Button pro Backup
- [ ] Bestätigung vor Rollback

---

## Implementiert

1. ✅ `deploy_to_prod.sh` erweitert (Backup vor Deploy, Auto-Cleanup)
2. ✅ `mira_rollback.sh` erstellt (interaktive Backup-Auswahl)
3. ✅ `mira_backup_cleanup.sh` erstellt (manueller Cleanup)
4. ⏸️ Optional: Frontend-Integration (später)

### Neue Scripts

| Script | Beschreibung |
|--------|-------------|
| `/opt/MIRA/tools/sh/deploy_to_prod.sh` | Backup vor Deploy, max 10 behalten |
| `/opt/MIRA/tools/sh/mira_rollback.sh` | Backup wiederherstellen |
| `/opt/MIRA/tools/sh/mira_backup_cleanup.sh` | Alte Backups manuell löschen |

---

**Zuletzt aktualisiert:** 2025-12-07 (Implementiert in Session 32)

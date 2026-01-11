# MIRA Widget Sound-Einstellungen - Sessionbericht

**Datum:** 12. Dezember 2025
**Dauer:** ca. 2 Stunden
**System:** MIRA Chat-Widget auf systemhaus-horst.de

---

## Zusammenfassung

Das Sound-Dropdown in den Company-Settings zeigte nach Seitenrefresh "Kein Ton" statt der gespeicherten Einstellung. Die Behebung erforderte mehrere Debugging-Runden aufgrund verketteter Probleme.

---

## Problem 1: HTTP 500 auf `/api/company/widget-greeting/info`

### Symptom
- In der Admin-Oberfläche (Company-Settings) wurde ein HTTP 500 Fehler angezeigt
- Das Sound-Dropdown zeigte immer "Kein Ton" an
- Browser-Konsole: `Failed to load resource: the server responded with a status of 500 (Internal Server Error)`

### Ursache
```python
# FEHLERHAFTER CODE in gateway.py (Zeile ~8897)
@app.get("/api/company/widget-greeting/info")
async def get_widget_greeting_info(admin: User = Depends(require_company_admin)):
    companies = load_companies()  # <-- FEHLER: Funktion existiert nicht!
```

Die Funktion `load_companies()` existierte nicht im Scope des Endpoints. Sie wurde vermutlich bei einem früheren Refactoring entfernt, aber der Endpoint wurde nicht aktualisiert.

### Lösung
```python
# KORRIGIERTER CODE
@app.get("/api/company/widget-greeting/info")
async def get_widget_greeting_info(
    admin: User = Depends(require_company_admin),
    company_mgr: CompanyManager = Depends(get_company_manager)  # <-- NEU: Dependency Injection
):
    company = company_mgr.get_company_by_slug(admin.company_slug)
    if not company:
        raise HTTPException(status_code=404, detail="Company not found")

    embed_settings = company.embed_settings or {}
    # ... Rest der Logik
```

### Betroffene Datei
- `/opt/MIRA/app/gateway.py` (Zeile ~8897)

---

## Problem 2: Falscher Widget Sound-Typ beim Kunden

### Symptom
- Nach Fix von Problem 1: Admin-UI zeigt korrekten Sound-Typ
- ABER: Widget beim Kunden spielt falschen Sound (oder keinen)
- Datenbank zeigt `sound: "boing"`, Widget liefert `SOUND_TYPE = "custom"`

### Ursache
**Verwirrung durch zwei verschiedene Companies:**

| Company | Embed-Key | Sound-Einstellung |
|---------|-----------|-------------------|
| DevoraXx | `emb_5dbf5bc4e2aae9dbbf7f4d60` | `sound: "boing"` |
| mira_support | `emb_d741f813472a624286a7ad74` | `sound: "custom"` |

Der Test wurde mit dem falschen Embed-Key durchgeführt (`mira_support` statt `DevoraXx`).

### Lösung
- Korrekte Identifikation des Embed-Keys für jede Company
- DevoraXx Webseite verwendet korrekterweise `emb_5dbf5bc4e2aae9dbbf7f4d60`
- Widget liefert nach Server-Neustart korrekt `SOUND_TYPE = "boing"`

---

## Problem 3: Browser-Caching

### Symptom
- Server liefert korrekten Sound-Typ
- Kunde hört trotzdem keinen/falschen Sound

### Ursache
Browser cached das Widget-JavaScript trotz `Cache-Control: no-cache` Header.

### Lösung
- Hard Refresh im Browser (Cmd+Shift+R / Ctrl+Shift+R)
- Oder: Browser-Cache komplett leeren

---

## Architektur-Übersicht

```
┌─────────────────────────────────────────────────────────────────┐
│                        MIRA System                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────────┐     ┌──────────────┐     ┌──────────────┐    │
│  │  Admin-UI    │────▶│  gateway.py  │────▶│ companies.   │    │
│  │  (Frontend)  │     │  (FastAPI)   │     │ json         │    │
│  └──────────────┘     └──────────────┘     └──────────────┘    │
│         │                    │                    ▲             │
│         │                    │                    │             │
│         ▼                    ▼                    │             │
│  /api/company/        /api/embed/           CompanyManager      │
│  widget-greeting/     widget.js             ._load_companies()  │
│  info                                                           │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│                     Kunden-Webseite                              │
├─────────────────────────────────────────────────────────────────┤
│  <Script src="https://systemhaus-horst.de/MIRA/api/embed/       │
│          widget.js?key=emb_5dbf5bc4e2aae9dbbf7f4d60" />         │
│                                                                  │
│  Widget liest SOUND_TYPE Variable und spielt entsprechenden     │
│  Sound beim Öffnen des Chat-Fensters                            │
└─────────────────────────────────────────────────────────────────┘
```

---

## Datenfluss: Sound-Einstellung

```
1. Admin wählt Sound "boing" im Dropdown
                    │
                    ▼
2. Frontend ruft POST /api/company/embed auf
   Body: { "sound": "boing", ... }
                    │
                    ▼
3. CompanyManager.update_embed_settings() speichert in companies.json
   { "embed_settings": { "sound": "boing", ... } }
                    │
                    ▼
4. Beim Widget-Abruf: GET /api/embed/widget.js?key=...
                    │
                    ▼
5. generate_widget_js() liest company.embed_settings
   und setzt: const SOUND_TYPE = "boing";
                    │
                    ▼
6. Widget spielt "boing" Sound beim Chat-Öffnen
```

---

## Betroffene Dateien

| Datei | Änderung |
|-------|----------|
| `/opt/MIRA/app/gateway.py` | Fix: Endpoint verwendet jetzt CompanyManager mit DI |
| `/opt/MIRA/services/mira_company.py` | Keine Änderung (nur untersucht) |
| `/opt/MIRA/data/companies/companies.json` | Datenquelle für Company-Einstellungen |

---

## Lessons Learned

1. **Dependency Injection nutzen**: Alle Endpoints sollten `CompanyManager` via `Depends()` verwenden, nicht eigenständige Funktionsaufrufe.

2. **Embed-Keys dokumentieren**: Bei mehreren Companies ist es wichtig zu wissen, welcher Key zu welcher Company gehört.

3. **Logging einbauen**: Der Endpoint hatte kein Logging - der Fehler war nur durch Server-Logs sichtbar.

4. **Browser-Caching beachten**: Auch bei `no-cache` Headern kann der Browser cachen. Immer Hard Refresh empfehlen.

5. **Separate Companies testen**: Beim Debugging sicherstellen, dass man die richtige Company testet.

---

## Präventionsmaßnahmen

### Empfohlen für die Zukunft:

1. **Unit Tests** für alle API-Endpoints hinzufügen
2. **Logging** in allen Endpoints implementieren
3. **Type Hints** und Validierung für bessere Fehlermeldungen
4. **Dokumentation** der Embed-Keys in Admin-UI anzeigen

---

## Aktueller Status

| Komponente | Status |
|------------|--------|
| Admin-UI Sound-Dropdown | ✅ Funktioniert |
| Sound-Speicherung | ✅ Funktioniert |
| Widget-Generierung | ✅ Funktioniert |
| Sound-Wiedergabe beim Kunden | ✅ Funktioniert (nach Cache-Clear) |

---

*Bericht erstellt am 12.12.2025, 20:55 Uhr*

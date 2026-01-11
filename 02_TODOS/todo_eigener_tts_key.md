# Company-Admin: Eigener ElevenLabs API-Key

**Priorität:** Mittel
**Aufwand:** 2-3 Stunden

## Ziel
Company-Admins sollen ihren eigenen ElevenLabs API-Key im Frontend eintragen können, damit sie die Sprachausgabe (TTS) unabhängig vom Systemhaus-Horst-Guthaben nutzen können.

## Aktueller Stand
- TTS funktioniert nur mit dem globalen ElevenLabs-Key
- Guthaben aufgebraucht -> alle Companies ohne Stimme
- Company-Admins haben keine Möglichkeit, eigenen Key einzutragen

## Umsetzung

### Backend (gateway.py + mira_company.py)
- [ ] Neues Feld `elevenlabs_api_key` in Company-Daten
- [ ] Endpoint `PUT /api/company/tts-settings` für Company-Admin
- [ ] TTS-Endpoint prüft: Company-Key vorhanden? -> nutzen, sonst System-Key
- [ ] Key verschlüsselt speichern (wie andere API-Keys)

### Frontend (index.html)
- [ ] Neue Sektion in Company-Settings: "Sprachausgabe"
- [ ] Input-Feld für ElevenLabs API-Key (masked)
- [ ] Test-Button: "Stimme testen"
- [ ] Toggle: TTS aktivieren/deaktivieren
- [ ] Voice-ID Auswahl (optional, Standard: Charlotte)

### Bonus
- [ ] Anzeige verbleibender Credits (ElevenLabs API)
- [ ] Warnung bei niedrigem Guthaben

## Notizen
- ElevenLabs Free Tier: 10.000 Zeichen/Monat
- Starter: $5/Monat für 30.000 Zeichen
- Speed-Parameter: 0.7-1.2 (nicht höher!)

---
Erstellt: 2025-12-13 (Session 48)

# MIRA Installer

**Priorität:** Mittel
**Aufwand:** 1-2 Tage

## Ziel
Ein Python-Script das MIRA auf einer frischen Debian-VM komplett installiert.

## Abfragen vorher
- Domain-Name
- Anthropic API Key
- OpenAI API Key (optional)
- ElevenLabs API Key (optional)
- SMTP-Daten
- Admin-User + Passwort
- Erste Company

## Automatisch
- System-Pakete
- Python venv + Dependencies
- Configs generieren
- Systemd-Services
- Nginx + SSL
- Erste Company + Admin anlegen
- Health-Check

## Notizen
- Python-Script bevorzugt
- Interaktiv mit Validierung
- Idempotent (mehrfach ausführbar)

---
Erstellt: 2025-12-13 (Session 47)

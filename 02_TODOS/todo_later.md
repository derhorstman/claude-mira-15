# Todo Later – Irgendwann-Liste

Hier landen Ideen und Aufgaben, die nicht brennen aber nicht vergessen werden sollen.

---

## Taskforce: Superadmin-Benachrichtigungen

**Status:** Geplant – Wichtig für Produktivbetrieb!

**Problem:** Als Superadmin keine sofortige Benachrichtigung wenn was schiefläuft.

**Lösungsansätze:**

1. **Telegram Bot**
   - Push-Alerts bei Server down
   - Hohe Error-Rate im Log
   - API-Limits erreicht (Token-Tracking)
   - Kunde meldet Problem
   - Bot-Token von @BotFather nötig

2. **DASBIEST Healthcheck erweitern**
   - Existiert bereits
   - Telegram-Anbindung hinzufügen
   - Schwellwerte definieren

3. **E-Mail-Alerts (Schnellste Option)**
   - SMTP ist konfiguriert
   - Alert-Mail an Superadmin
   - Fallback wenn Telegram nicht geht

**Nächste Schritte:**
- [ ] Telegram Bot bei @BotFather erstellen
- [ ] Bot-Token + Chat-ID konfigurieren
- [ ] Alert-Service implementieren (`/opt/MIRA/services/mira_alerts.py`)
- [ ] In DASBIEST Healthcheck integrieren

---

## Bot-Schutz erweitern (falls nötig)

**Status:** Aktuell kein Problem – beobachten

Wenn Spam trotz Rate Limiting + Honeypot durchkommt:
- **Turnstile (Cloudflare)** wäre erste Wahl
- Unsichtbar, DSGVO-freundlich, kostenlos
- Kein nerviges Ampel-Klicken für User

---

## Live-Chat Erweiterungen (Phase 3 - optional)

**Status:** Nice-to-have

- [ ] Eskalations-Benachrichtigung (Kunde braucht Hilfe)
- [ ] KI + Mensch parallel (Mensch korrigiert/ergänzt)

---

## MIRA Marke anmelden (DPMA)

**Status:** Offen

- https://direkt.dpma.de/marke/
- Klassen: 9 + 42
- Kosten: ~290€
- EVY separat schützen?

---

## Code-Qualität: Schwachstellen beheben

**Status:** Geplant – Wertsteigerung +25-40%

Aus der Quellcode-Bewertung identifizierte Schwachstellen:

### 1. Frontend-Refactoring (Priorität: Mittel)
- [ ] index.html aufteilen (14.000 Zeilen → Komponenten)
- [ ] JavaScript in separate Module extrahieren
- [ ] CSS in eigene Dateien auslagern
- **Wertsteigerung:** +5-10%

### 2. Build-Pipeline (Priorität: Hoch)
- [ ] Webpack oder Vite einrichten
- [ ] Minification + Bundling
- [ ] Source Maps für Debugging
- [ ] Hot-Reload für Entwicklung
- **Wertsteigerung:** +5-10%

### 3. Unit-Tests (Priorität: Hoch)
- [ ] pytest für Backend einrichten
- [ ] Tests für kritische Services (mira_users, mira_company, mira_brain)
- [ ] API-Endpoint-Tests (FastAPI TestClient)
- [ ] Coverage-Report generieren
- **Wertsteigerung:** +10-15%

### 4. Vendor Lock-in reduzieren (Priorität: Niedrig)
- [ ] Alternative TTS-Option (z.B. Google TTS, lokales Modell)
- [ ] Alternative Wake-Word (z.B. Snowboy, Vosk)
- [ ] Abstraktion für einfachen Provider-Wechsel
- **Wertsteigerung:** +5%

### Empfohlene Reihenfolge
1. Unit-Tests → sichert Qualität bei Refactoring
2. Build-Pipeline → ermöglicht sauberes Frontend-Refactoring
3. Frontend-Refactoring → dann erst index.html aufteilen
4. Vendor Lock-in → kann später erfolgen

---

## Notizen

*Hier Ideen parken, die beim Arbeiten aufkommen:*

-

---

**Zuletzt aktualisiert:** 2025-12-07 (Session 38)

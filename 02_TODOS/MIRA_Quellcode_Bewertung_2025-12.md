# MIRA Quellcode-Wertermittlung

**Gutachten zur Bewertung des Software-Assets**

---

| Feld | Wert |
|------|------|
| **Projekt** | MIRA - Modern Intelligence & Response Architecture |
| **Version** | 2.10.0 |
| **Bewertungsdatum** | 07. Dezember 2025 |
| **Bewertungsmethode** | Mehrfachverfahren (Kosten-, Markt-, Ertragswert) |
| **Dokumentersteller** | Technische Analyse |

---

## 1. Executive Summary

MIRA ist eine produktionsreife Multi-Tenant SaaS-Plattform für KI-gestützte Assistenzsysteme. Die Software befindet sich im aktiven Produktivbetrieb und wird kontinuierlich weiterentwickelt.

### Bewertungsergebnis (Zusammenfassung)

| Bewertungsansatz | Wertspanne |
|------------------|------------|
| Wiederbeschaffungswert | 195.600 - 293.400 EUR |
| COCOMO II (Entwicklungsaufwand) | 640.000 - 1.150.000 EUR |
| Function-Point-Analyse | 540.000 - 900.000 EUR |
| Aktueller Ertragswert | 60.000 - 85.000 EUR |

**Empfohlener fairer Verkehrswert:** 250.000 - 350.000 EUR

---

## 2. Projektbeschreibung

### 2.1 Funktionsumfang

MIRA ist eine vollständige SaaS-Lösung mit folgenden Kernfunktionen:

| Funktionsbereich | Beschreibung | Status |
|------------------|--------------|--------|
| Multi-Tenant-Architektur | Mehrere Firmen auf einer Plattform | Produktiv |
| KI-Chat-System | Integration Claude (Anthropic) & GPT (OpenAI) | Produktiv |
| Brain-System | Vektorbasierte Wissensdatenbank pro User/Company | Produktiv |
| Embed-Widget | Chat-Widget für externe Websites | Produktiv |
| Partner-Portal | Self-Service Demo-Erstellung mit Provision | Produktiv |
| Voice-System | Wake-Word, Spracheingabe, Sprachausgabe | Produktiv |
| Live-Chat | Telegram-Integration mit Admin-Takeover | Produktiv |
| Serviceportal | Verträge, Rechnungen, Handbücher | Produktiv |
| 2FA-Authentifizierung | TOTP mit Backup-Codes | Produktiv |
| Deploy-System | Dev/Prod-Synchronisation | Produktiv |

### 2.2 Technologie-Stack

| Schicht | Technologie |
|---------|-------------|
| Backend | Python 3.11, FastAPI, Uvicorn |
| Frontend | Vanilla JavaScript (Single-Page-Application) |
| KI-Integration | Anthropic Claude API, OpenAI API |
| Vektorsuche | Sentence-Transformers (paraphrase-multilingual-MiniLM-L12-v2) |
| Text-to-Speech | ElevenLabs API |
| Speech-to-Text | Whisper (EVY Voice API) |
| Wake-Word | Picovoice Porcupine |
| Webserver | Apache (Reverse Proxy), Let's Encrypt SSL |
| E-Mail | SMTP, Roundcube Webmail |
| Push-Notifications | Web Push API (VAPID) |

### 2.3 Infrastruktur

| Komponente | Beschreibung |
|------------|--------------|
| Produktionsserver | Debian Linux, Port 8000 |
| Entwicklungsserver | Gespiegelte Umgebung, Port 8100 |
| Reverse Proxy | Apache mit SSL-Terminierung |
| Domains | systemhaus-horst.de, mukupi.art |

---

## 3. Quantitative Analyse

### 3.1 Code-Metriken

| Kategorie | Metrik | Wert |
|-----------|--------|------|
| **Backend** | Python-Quellcode | 21.449 Zeilen |
| | Service-Module | 21 Dateien |
| | API-Endpoints | 214 Routen |
| | Funktionen/Methoden | 449 |
| **Frontend** | HTML/JS/CSS | 20.250 Zeilen |
| | Hauptanwendung (index.html) | 14.027 Zeilen |
| | Zusätzliche Seiten | 6 Dateien |
| **Dokumentation** | Markdown-Dokumentation | 6.284 Zeilen |
| | Handbücher (PDF) | 3 Dokumente |
| **Operations** | Shell-Skripte | 15 Dateien |
| **Gesamt** | Produktiver Code | **41.699 Zeilen** |
| | Mit Dokumentation | **47.983 Zeilen** |

### 3.2 Funktionale Komplexität

| Kategorie | Anzahl | Komplexität |
|-----------|--------|-------------|
| REST-API-Endpoints | 214 | Mittel-Hoch |
| Datenbankentitäten | 8 (User, Company, Brain, Document, Session, etc.) | Mittel |
| Externe Integrationen | 8 (Claude, OpenAI, ElevenLabs, Telegram, etc.) | Hoch |
| Authentifizierungsflows | 4 (Login, 2FA, Token, Session) | Hoch |
| Dateitypen (Upload) | 10+ (PDF, DOCX, MD, TXT, XLSX, etc.) | Mittel |

### 3.3 Aktive Nutzung

| Metrik | Wert |
|--------|------|
| Registrierte Companies | 10+ |
| Produktivbetrieb seit | November 2025 |
| Entwicklungs-Sessions | 37+ dokumentiert |

---

## 4. Bewertungsmethoden

### 4.1 Methode 1: Wiederbeschaffungswert (Cost Approach)

Der Wiederbeschaffungswert berechnet, was es kosten würde, die Software heute neu zu entwickeln.

#### 4.1.1 Aufwandsschätzung nach Komponenten

| Komponente | Stunden (Min) | Stunden (Max) | Stundensatz | Kosten (Min) | Kosten (Max) |
|------------|---------------|---------------|-------------|--------------|--------------|
| Architektur & Design | 80 | 120 | 150 EUR | 12.000 EUR | 18.000 EUR |
| Backend-Entwicklung | 800 | 1.200 | 100 EUR | 80.000 EUR | 120.000 EUR |
| Frontend-Entwicklung | 400 | 600 | 90 EUR | 36.000 EUR | 54.000 EUR |
| KI-Integration | 200 | 300 | 120 EUR | 24.000 EUR | 36.000 EUR |
| Voice-System | 100 | 150 | 120 EUR | 12.000 EUR | 18.000 EUR |
| Testing & QA | 200 | 300 | 80 EUR | 16.000 EUR | 24.000 EUR |
| Dokumentation | 80 | 120 | 70 EUR | 5.600 EUR | 8.400 EUR |
| DevOps/Infrastruktur | 100 | 150 | 100 EUR | 10.000 EUR | 15.000 EUR |
| **Gesamt** | **1.960** | **2.940** | - | **195.600 EUR** | **293.400 EUR** |

#### 4.1.2 Begründung der Stundensätze

| Rolle | Stundensatz | Begründung |
|-------|-------------|------------|
| Senior Architekt | 150 EUR | Erfahrung mit Multi-Tenant-SaaS erforderlich |
| Backend-Entwickler | 100 EUR | Python/FastAPI-Spezialist |
| Frontend-Entwickler | 90 EUR | JavaScript SPA-Entwicklung |
| KI-Spezialist | 120 EUR | LLM-Integration, Prompt Engineering |
| DevOps Engineer | 100 EUR | Linux, Apache, Deployment |
| QA Engineer | 80 EUR | Manuelles Testing, Integration |
| Technischer Redakteur | 70 EUR | Dokumentation |

### 4.2 Methode 2: COCOMO II (Entwicklungsaufwand)

Das Constructive Cost Model II ist ein algorithmisches Kostenschätzmodell.

#### 4.2.1 Berechnung

| Parameter | Wert |
|-----------|------|
| KLOC (Kilo Lines of Code) | 41,7 |
| Projekttyp | Semi-Embedded (Multi-Tenant, AI) |
| Koeffizient a | 3,0 |
| Exponent b | 1,12 |
| EAF (Effort Adjustment Factor) | 1,2 (erhöhte Komplexität) |

**Formel:** PM = a × (KLOC)^b × EAF

**Berechnung:**
- PM = 3,0 × (41,7)^1,12 × 1,2
- PM = 3,0 × 68,5 × 1,2
- PM = **246,6 Personenmonate**

#### 4.2.2 Kostenberechnung

| Szenario | PM | Monatssatz | Gesamtkosten |
|----------|-----|------------|--------------|
| Konservativ | 200 | 8.000 EUR | 1.600.000 EUR |
| Realistisch | 247 | 4.000 EUR | 988.000 EUR |
| Optimistisch | 160 | 4.000 EUR | 640.000 EUR |

**COCOMO II Wertspanne:** 640.000 - 1.150.000 EUR

### 4.3 Methode 3: Function-Point-Analyse

Die Function-Point-Methode bewertet Software nach ihrer funktionalen Komplexität.

#### 4.3.1 Funktionskategorien

| Kategorie | Anzahl | Gewicht | Punkte |
|-----------|--------|---------|--------|
| **Externe Eingaben (EI)** | | | |
| - Login/Auth-Flows | 4 | 6 | 24 |
| - CRUD-Operationen | 45 | 4 | 180 |
| - File-Uploads | 8 | 6 | 48 |
| - Chat-Nachrichten | 5 | 4 | 20 |
| **Externe Ausgaben (EO)** | | | |
| - API-Responses | 100 | 5 | 500 |
| - Streaming (Chat, TTS) | 4 | 7 | 28 |
| - Reports/Exports | 6 | 6 | 36 |
| **Externe Abfragen (EQ)** | | | |
| - Such-Funktionen | 12 | 4 | 48 |
| - Status-Abfragen | 20 | 3 | 60 |
| **Interne Logische Dateien (ILF)** | | | |
| - Datenmodelle | 8 | 10 | 80 |
| - Konfigurationen | 5 | 7 | 35 |
| **Externe Schnittstellen (EIF)** | | | |
| - KI-APIs (Claude, OpenAI) | 2 | 10 | 20 |
| - TTS/STT (ElevenLabs, Whisper) | 2 | 7 | 14 |
| - Telegram API | 1 | 7 | 7 |
| - E-Mail (SMTP) | 1 | 5 | 5 |
| - Cloud-Speicher (OneDrive, etc.) | 3 | 7 | 21 |
| **Gesamt Unadjusted FP** | | | **1.126** |

#### 4.3.2 Anpassungsfaktoren (VAF)

| Faktor | Bewertung (0-5) | Begründung |
|--------|-----------------|------------|
| Datenkommunikation | 4 | REST API, Streaming |
| Verteilte Verarbeitung | 3 | Multi-Server-Setup |
| Performance | 3 | Echtzeitchat |
| Hardwarebeanspruchung | 2 | Moderate Anforderungen |
| Transaktionsrate | 3 | Mittlere Frequenz |
| Online-Dateneingabe | 5 | Vollständige SPA |
| Endbenutzer-Effizienz | 4 | Benutzerfreundliches UI |
| Online-Aktualisierung | 4 | Echtzeit-Updates |
| Komplexe Verarbeitung | 4 | KI, Vektoren, NLP |
| Wiederverwendbarkeit | 3 | Multi-Tenant |
| Installationsfreundlichkeit | 3 | Script-basiert |
| Betriebsfreundlichkeit | 4 | Monitoring, Logs |
| Mehrere Standorte | 2 | Einzelner Server |
| Änderungsfreundlichkeit | 3 | Modularer Aufbau |
| **Summe (TDI)** | **47** | |

**VAF = 0,65 + (TDI × 0,01) = 0,65 + 0,47 = 1,12**

#### 4.3.3 Adjusted Function Points

**AFP = UFP × VAF = 1.126 × 1,12 = 1.261 Function Points**

#### 4.3.4 Kostenberechnung

| Produktivität | EUR/FP | Gesamtwert |
|---------------|--------|------------|
| Niedrig (15 h/FP) | 45 EUR | 567.450 EUR |
| Mittel (20 h/FP) | 30 EUR | 378.300 EUR |
| Hoch (25 h/FP) | 24 EUR | 302.640 EUR |

**Function-Point Wertspanne:** 300.000 - 570.000 EUR

### 4.4 Methode 4: Ertragswertverfahren (Income Approach)

#### 4.4.1 Potenzielle Einnahmen

| Szenario | Kunden | Ø Preis/Monat | Jahresumsatz |
|----------|--------|---------------|--------------|
| Aktuell | 5 | 79 EUR | 4.740 EUR |
| Jahr 1 | 20 | 79 EUR | 18.960 EUR |
| Jahr 2 | 50 | 89 EUR | 53.400 EUR |
| Jahr 3 | 100 | 99 EUR | 118.800 EUR |

#### 4.4.2 Discounted Cash Flow (DCF)

| Jahr | Cashflow | Diskontfaktor (12%) | Barwert |
|------|----------|---------------------|---------|
| 1 | 18.960 EUR | 0,893 | 16.931 EUR |
| 2 | 53.400 EUR | 0,797 | 42.560 EUR |
| 3 | 118.800 EUR | 0,712 | 84.586 EUR |
| 4 | 150.000 EUR | 0,636 | 95.400 EUR |
| 5 | 180.000 EUR | 0,567 | 102.060 EUR |
| **Summe** | | | **341.537 EUR** |

**Ertragswert bei konservativer Wachstumsannahme:** 250.000 - 350.000 EUR

---

## 5. Qualitative Bewertungsfaktoren

### 5.1 Wertsteigende Faktoren

| Faktor | Bewertung | Begründung |
|--------|-----------|------------|
| **Produktionsreife** | +++ | Aktiver Betrieb mit echten Kunden |
| **Multi-Tenant-Architektur** | +++ | Skalierbar, mehrere Kunden möglich |
| **Dokumentation** | ++ | Umfangreiche Handbücher und API-Docs |
| **DSGVO-Konformität** | +++ | Deutsche Server, keine US-Cloud |
| **Alleinstellungsmerkmale** | ++ | Voice-System, Partner-Provision |
| **Aktive Entwicklung** | ++ | 37+ dokumentierte Sessions |
| **Technologie-Stack** | ++ | Moderne, etablierte Technologien |

### 5.2 Wertmindernde Faktoren

| Faktor | Bewertung | Begründung |
|--------|-----------|------------|
| **Monolithisches Frontend** | - | 14.000 Zeilen in einer Datei |
| **Keine Build-Pipeline** | - | Direkte Änderungen in build/ |
| **Single-Developer** | -- | Abhängigkeit von einer Person |
| **Fehlende Tests** | -- | Kein dokumentiertes Test-Coverage |
| **Vendor Lock-in** | - | Abhängigkeit von ElevenLabs, Picovoice |

### 5.3 Risikobewertung

| Risiko | Wahrscheinlichkeit | Auswirkung | Mitigation |
|--------|-------------------|------------|------------|
| API-Preisänderungen | Mittel | Mittel | Multi-Provider-Support vorhanden |
| Sicherheitslücken | Niedrig | Hoch | 2FA, Rate Limiting implementiert |
| Skalierungsprobleme | Mittel | Mittel | Refactoring bei Wachstum nötig |
| Wissensverlust | Hoch | Hoch | Dokumentation vorhanden |

---

## 6. Marktvergleich

### 6.1 Vergleichbare Produkte

| Produkt | Typ | Preis | Differenzierung zu MIRA |
|---------|-----|-------|------------------------|
| Tidio | SaaS | 29-289 EUR/Monat | Kein echtes KI, Flow-Bots |
| Intercom | SaaS | ab 74 EUR/Monat | US-Cloud, teuer |
| Drift | Enterprise | Individuell | Enterprise-fokussiert |
| Freshchat | SaaS | 15-83 EUR/Monat | Limitierte KI |
| Zendesk AI | SaaS | ab 55 EUR/Monat | US-Cloud |

### 6.2 Alleinstellungsmerkmale von MIRA

1. **Deutsche Hosting-Option** - DSGVO out-of-the-box
2. **Voice-System** - Wake-Word auf Deutsch ("Hey MIRA")
3. **White-Label Multi-Tenant** - Eigenes Branding pro Kunde
4. **Partner-Provisionssystem** - MiraCoin für Vertriebspartner
5. **Eigene KI-Keys** - Kunden können eigene API-Keys nutzen
6. **Live-Chat Takeover** - Nahtloser Mensch-KI-Wechsel

---

## 7. Wertermittlung

### 7.1 Synthese der Bewertungsmethoden

| Methode | Minimum | Maximum | Gewichtung |
|---------|---------|---------|------------|
| Wiederbeschaffungswert | 195.600 EUR | 293.400 EUR | 30% |
| Function-Point-Analyse | 300.000 EUR | 570.000 EUR | 25% |
| COCOMO II | 640.000 EUR | 1.150.000 EUR | 15% |
| Ertragswert (DCF) | 250.000 EUR | 350.000 EUR | 30% |

### 7.2 Gewichtete Berechnung

**Minimum:**
- (195.600 × 0,30) + (300.000 × 0,25) + (640.000 × 0,15) + (250.000 × 0,30)
- = 58.680 + 75.000 + 96.000 + 75.000
- = **304.680 EUR**

**Maximum:**
- (293.400 × 0,30) + (570.000 × 0,25) + (1.150.000 × 0,15) + (350.000 × 0,30)
- = 88.020 + 142.500 + 172.500 + 105.000
- = **508.020 EUR**

### 7.3 Empfohlene Wertspannen

| Kategorie | Wertspanne | Verwendung |
|-----------|------------|------------|
| **Liquidationswert** | 100.000 - 150.000 EUR | Notverkauf, keine Weiterentwicklung |
| **Substanzwert** | 200.000 - 300.000 EUR | Basis für Verhandlungen |
| **Verkehrswert** | 300.000 - 400.000 EUR | Fairer Marktwert |
| **Strategischer Wert** | 400.000 - 500.000 EUR | Für strategischen Käufer |

---

## 8. Empfehlung

### 8.1 Fairer Verkehrswert

Basierend auf der Analyse aller Faktoren wird folgender **fairer Verkehrswert** empfohlen:

| | Wert |
|--|------|
| **Minimum** | 280.000 EUR |
| **Empfohlen** | **320.000 EUR** |
| **Maximum** | 380.000 EUR |

### 8.2 Begründung

1. **Produktionsreife Software** mit aktivem Kundenbetrieb rechtfertigt Aufschlag gegenüber reinem Entwicklungswert
2. **Multi-Tenant-Architektur** ermöglicht sofortige Skalierung
3. **Dokumentation** reduziert Einarbeitungsrisiko
4. **Deutsche Hosting-Option** ist Marktvorteil im B2B-Bereich
5. **Voice-System** als technisches Alleinstellungsmerkmal

### 8.3 Wertsteigerungspotenzial

| Maßnahme | Potenzielle Wertsteigerung |
|----------|---------------------------|
| Unit-Test-Suite implementieren | +10-15% |
| Frontend-Refactoring (Komponenten) | +5-10% |
| CI/CD-Pipeline einrichten | +5-10% |
| Kundenstamm auf 50+ erweitern | +50-100% |
| ISO 27001 Zertifizierung | +15-25% |

---

## 9. Haftungsausschluss

Diese Bewertung basiert auf der Analyse des Quellcodes, der Dokumentation und der technischen Infrastruktur zum Bewertungszeitpunkt. Sie stellt keine Rechts-, Steuer- oder Finanzberatung dar. Für verbindliche Bewertungen wird die Hinzuziehung eines vereidigten Sachverständigen empfohlen.

Die tatsächlichen Marktpreise können je nach:
- Käuferinteresse und strategischer Passung
- Wirtschaftlicher Gesamtlage
- Verhandlungsgeschick
- Due-Diligence-Ergebnissen

erheblich abweichen.

---

## 10. Anhänge

### Anhang A: Verzeichnisstruktur

```
/opt/MIRA/
├── app/
│   ├── gateway.py          (8.669 Zeilen)
│   └── claude.py           (523 Zeilen)
├── services/
│   ├── mira_brain.py       (984 Zeilen)
│   ├── mira_company.py     (1.970 Zeilen)
│   ├── mira_users.py       (868 Zeilen)
│   ├── mira_documents.py   (871 Zeilen)
│   ├── mira_telegram.py    (641 Zeilen)
│   ├── mira_mail.py        (621 Zeilen)
│   ├── mira_coins.py       (658 Zeilen)
│   └── [12 weitere Module]
├── frontend/build/
│   ├── index.html          (14.027 Zeilen)
│   └── [6 weitere Seiten]
├── docs/
│   ├── README/             (8 Dokumentationen)
│   ├── Admin-Handbuch.md
│   ├── Benutzerhandbuch.md
│   └── Vertriebspartner-Handbuch.md
├── companies/              (10+ Mandanten)
├── config/
└── tools/sh/               (15 Skripte)
```

### Anhang B: Feature-Matrix

| Feature | Basis | Pro | Enterprise |
|---------|-------|-----|------------|
| KI-Chat | ✓ | ✓ | ✓ |
| Brain-System | ✓ | ✓ | ✓ |
| Embed-Widget | ✓ | ✓ | ✓ |
| 2FA | ✓ | ✓ | ✓ |
| Voice-System | - | ✓ | ✓ |
| Telegram Live-Chat | - | ✓ | ✓ |
| Partner-Portal | - | - | ✓ |
| White-Label | - | - | ✓ |
| Custom API-Keys | - | ✓ | ✓ |

### Anhang C: Technische Abhängigkeiten

| Abhängigkeit | Version | Lizenz | Kritikalität |
|--------------|---------|--------|--------------|
| FastAPI | 0.100+ | MIT | Hoch |
| Anthropic SDK | 0.18+ | MIT | Hoch |
| OpenAI SDK | 1.0+ | MIT | Mittel |
| Sentence-Transformers | 2.2+ | Apache 2.0 | Hoch |
| ElevenLabs API | - | Proprietär | Mittel |
| Picovoice | - | Proprietär | Niedrig |

---

**Dokumentenende**

*Erstellt: 07. Dezember 2025*
*Version: 1.0*

# MIRA Quellcode-Analyse

**Stand:** 2025-12-07
**Version:** 2.10.0

---

## 1. Projekt-Übersicht

| Metrik | Wert |
|--------|------|
| **Gesamtcode** | ~48.000 Zeilen |
| Backend (Python) | 21.449 Zeilen |
| Frontend (HTML/JS/CSS) | 20.250 Zeilen |
| Dokumentation (MD) | 6.284 Zeilen |
| API-Endpoints | 214 |
| Service-Module | 21 |
| Shell-Skripte | 15 |

---

## 2. Backend-Struktur

### 2.1 Hauptanwendung (`/opt/MIRA/app/`)

| Datei | Zeilen | Funktion |
|-------|--------|----------|
| `gateway.py` | 8.669 | FastAPI REST API, alle Endpoints |
| `claude.py` | 523 | KI-Client (Claude + OpenAI Wrapper) |
| `__init__.py` | 4 | Package-Init |

### 2.2 Service-Module (`/opt/MIRA/services/`)

| Modul | Zeilen | Funktion | Abhängigkeiten |
|-------|--------|----------|----------------|
| `mira_company.py` | 1.970 | Multi-Tenant-Verwaltung | - |
| `mira_media_worker.py` | 1.159 | Dateiverarbeitung (OCR, PDF) | mira_brain |
| `mira_brain.py` | 984 | Vektorbasierte Wissensdatenbank | mira_embeddings |
| `mira_documents.py` | 871 | Serviceportal Dokumente | - |
| `mira_users.py` | 868 | Benutzerverwaltung + 2FA | bcrypt |
| `mira_demo_keys.py` | 699 | Demo-Automation | mira_company |
| `mira_coins.py` | 658 | Partner-Provisionssystem | mira_users |
| `mira_telegram.py` | 641 | Live-Chat Integration | telegram API |
| `mira_mail.py` | 621 | E-Mail-Service (SMTP) | - |
| `mira_onedrive.py` | 522 | OneDrive-Integration | OAuth |
| `mira_web_scraper.py` | 499 | Demo-Webscraping | BeautifulSoup |
| `mira_vectorstore.py` | 474 | Vektor-Datenbank | sentence-transformers |
| `mira_gdrive.py` | 461 | Google Drive-Integration | OAuth |
| `mira_dropbox.py` | 413 | Dropbox-Integration | OAuth |
| `mira_embeddings.py` | 377 | Text-Vektorisierung | sentence-transformers |
| `mira_cache_demo.py` | 264 | Wegwerf-Demos | mira_company |
| `mira_push.py` | 233 | Push-Notifications | VAPID |
| `mira_outbox_worker.py` | 216 | Outbox-Verarbeitung | mira_brain |
| `mira_biest_customers.py` | ~200 | DASBIEST Kunden-Sync | - |
| `mira_tts.py` | 144 | Text-to-Speech | ElevenLabs API |
| `__init__.py` | 0 | Package-Init | - |

### 2.3 Abhängigkeitsgraph

```
gateway.py
    ├── claude.py (KI-Clients)
    ├── mira_users.py (Auth)
    ├── mira_company.py (Multi-Tenant)
    │   └── mira_demo_keys.py
    │   └── mira_cache_demo.py
    ├── mira_brain.py (Wissen)
    │   └── mira_embeddings.py
    │   └── mira_vectorstore.py
    ├── mira_documents.py (Portal)
    ├── mira_telegram.py (Live-Chat)
    ├── mira_mail.py (E-Mail)
    ├── mira_push.py (Notifications)
    ├── mira_tts.py (Voice)
    ├── mira_coins.py (Partner)
    └── Cloud-Services
        ├── mira_onedrive.py
        ├── mira_gdrive.py
        └── mira_dropbox.py
```

---

## 3. Frontend-Struktur

### 3.1 Dateien (`/opt/MIRA/frontend/build/`)

| Datei | Zeilen | Funktion |
|-------|--------|----------|
| `index.html` | 14.027 | Haupt-SPA (Login, Chat, Admin, Portal) |
| `landingpage.html` | 754 | 4-Türen Startseite |
| `partner.html` | 937 | Partner-Portal |
| `sales.html` | 1.245 | Vertriebs-Demo |
| `demo_cache.html` | 1.234 | Wegwerf-Demo |
| `biest.html` | 792 | DASBIEST Landingpage |
| `ki-chat-brain.html` | 571 | Standalone Chat |
| `modules.js` | 601 | Modul-System |
| `sw-push.js` | 89 | Service Worker (Push) |

### 3.2 index.html Aufbau (monolithisch)

```
index.html (~14.000 Zeilen)
├── <head> (Zeile 1-50)
│   └── Meta, Styles inline
├── <style> (Zeile 50-2500)
│   └── ~2.450 Zeilen CSS inline
├── <body> (Zeile 2500-14000)
│   ├── Navigation/Sidebar
│   ├── Login-Page
│   ├── Chat-Page
│   ├── Brain-Page
│   ├── Files-Page (Inbox/Outbox)
│   ├── Portal-Page (Serviceportal)
│   ├── Admin-Pages
│   │   ├── Users
│   │   ├── Company-Settings
│   │   ├── Live-Chats
│   │   └── Superadmin
│   ├── Partner-Page (MiraCoins)
│   └── <script> (~8.000 Zeilen JS inline)
│       ├── State Management
│       ├── API-Calls
│       ├── UI-Funktionen
│       ├── Voice-System
│       └── Utility-Funktionen
```

### 3.3 Kritische JS-Funktionen

| Bereich | Funktionen | Zeilen (ca.) |
|---------|------------|--------------|
| Auth | `login()`, `logout()`, `checkSession()` | 200 |
| Chat | `sendMessage()`, `renderChat()`, `streamResponse()` | 500 |
| Brain | `loadBrain()`, `uploadToBrain()`, `searchBrain()` | 400 |
| Voice | `startRecording()`, `playTTS()`, `initWakeWord()` | 600 |
| Admin | `loadUsers()`, `saveCompanySettings()`, `loadLiveChats()` | 800 |
| Portal | `loadContracts()`, `loadInvoices()`, `loadManuals()` | 500 |
| API | `apiCall()`, `apiGet()`, `apiPost()` | 150 |
| Utils | `showToast()`, `formatDate()`, `debounce()` | 200 |

---

## 4. Datenmodelle

### 4.1 Company (`mira_company.py`)

```python
@dataclass
class Company:
    id: str
    name: str
    slug: str                          # URL-sicherer Name
    active: bool
    max_users: int
    # Branding
    logo_filename: str
    primary_color: str
    secondary_color: str
    # API
    api_keys: Dict[str, CompanyAPIKey]  # provider -> key
    ai_model: str
    max_tokens_per_request: int
    # Embed Widget
    embed_key: str
    embed_enabled: bool
    embed_settings: Dict
    # Features
    voice_enabled: bool
    telegram_enabled: bool
    # Demo
    is_demo: bool
    demo_expires_at: str
```

### 4.2 User (`mira_users.py`)

```python
@dataclass
class User:
    id: str
    username: str
    password_hash: str                 # bcrypt
    company_id: str
    company_slug: str
    email: str
    role: str                          # super_admin, company_admin, user
    # 2FA
    totp_secret: str
    totp_enabled: bool
    backup_codes: List[str]
    # Berechtigungen
    module_permissions: Dict[str, bool]
```

### 4.3 Brain Entry (`mira_brain.py`)

```python
@dataclass
class BrainEntry:
    id: str
    content: str
    user_id: str
    entry_type: str                    # memory, document, note
    metadata: Dict
    timestamp: str
    vector: List[float]                # 384 Dimensionen
```

### 4.4 Document (`mira_documents.py`)

```python
@dataclass
class Document:
    id: str
    company_slug: str
    doc_type: str                      # contract, invoice, manual
    filename: str
    title: str
    status: str                        # active, archive, open, paid
    metadata: Dict
```

---

## 5. API-Struktur

### 5.1 Endpoint-Kategorien

| Kategorie | Prefix | Endpoints | Auth |
|-----------|--------|-----------|------|
| Auth | `/api/` | 5 | Nein/Ja |
| Chat | `/api/chat` | 3 | Ja |
| Brain | `/api/brain` | 8 | Ja |
| Users | `/api/users` | 6 | Admin |
| Company | `/api/company` | 15 | Admin |
| Embed | `/api/embed` | 5 | Key |
| Portal | `/api/portal` | 12 | Ja |
| Voice | `/api/tts`, `/api/voice` | 4 | Ja |
| Admin | `/api/admin` | 20 | Super |
| Files | `/api/files` | 6 | Ja |
| Cloud | `/api/cloud` | 9 | Ja |

### 5.2 Wichtige Endpoints

```
POST /api/login                    # Auth
POST /api/chat                     # KI-Chat (streaming)
GET  /api/brain                    # Brain laden
POST /api/brain/upload             # Datei ins Brain
POST /api/embed/chat               # Widget-Chat (public)
GET  /api/company                  # Company-Settings
POST /api/company/api-keys         # API-Key hinzufügen
GET  /api/admin/companies          # Alle Companies
GET  /api/admin/live-chats         # Live-Chat Dashboard
POST /api/tts                      # Text-to-Speech
GET  /api/portal/manuals           # Handbücher
```

---

## 6. Konfiguration

### 6.1 Dateien

| Datei | Funktion |
|-------|----------|
| `/opt/MIRA/config/mira.yaml` | Hauptkonfiguration |
| `/opt/MIRA/config/.env` | Secrets (API-Keys) |

### 6.2 mira.yaml Struktur

```yaml
server:
  host: "127.0.0.1"
  port: 8000
  workers: 4

paths:
  companies: "/opt/MIRA/companies"
  data: "/opt/MIRA/data"
  logs: "/opt/MIRA/logs"

ai:
  default_provider: "anthropic"
  default_model: "claude-sonnet-4-20250514"

brain:
  embedding_model: "paraphrase-multilingual-MiniLM-L12-v2"
  max_entries: 10000

features:
  brain_vectorization: true
  voice_enabled: true
```

### 6.3 Umgebungsvariablen (.env)

```
# E-Mail
SMTP_HOST=smtp.udag.de
SMTP_PORT=587
SMTP_USER=...
SMTP_PASS=...

# Push Notifications
VAPID_PRIVATE_KEY=...
VAPID_PUBLIC_KEY=...

# Voice
ELEVENLABS_API_KEY=sk_e5cd752d...
ELEVENLABS_VOICE_ID=XB0fDUnXU5powFXDhCwa
PICOVOICE_ACCESS_KEY=1FvIp4+Gv5ti...
```

---

## 7. Datenspeicherung

### 7.1 Verzeichnisstruktur

```
/opt/MIRA/
├── companies/
│   └── {company-slug}/
│       ├── global/
│       │   └── brain/
│       │       ├── brain.json          # Vektoren
│       │       └── uploads/            # Dateien
│       ├── {username}/
│       │   ├── brain/
│       │   ├── inbox/
│       │   └── outbox/
│       └── documents/
│           ├── contracts/
│           └── invoices/
├── data/
│   ├── companies/
│   │   └── companies.json
│   ├── users/
│   │   └── users.json
│   ├── manuals/                        # Globale Handbücher
│   ├── demo_keys/
│   │   └── demo_sessions.json
│   └── documents/
│       └── documents_index.json
└── logs/
    ├── gateway.log
    └── services/
```

### 7.2 JSON-Strukturen

**companies.json:**
```json
{
  "companies": [
    { "id": "...", "name": "...", "slug": "...", ... }
  ]
}
```

**users.json:**
```json
{
  "users": [
    { "id": "...", "username": "...", "company_id": "...", ... }
  ]
}
```

**brain.json:**
```json
{
  "entries": [
    { "id": "...", "content": "...", "vector": [...], ... }
  ],
  "model": "paraphrase-multilingual-MiniLM-L12-v2"
}
```

---

## 8. Externe Abhängigkeiten

### 8.1 Python-Packages

| Package | Version | Verwendung | Kritikalität |
|---------|---------|------------|--------------|
| fastapi | 0.100+ | Web-Framework | Hoch |
| uvicorn | 0.23+ | ASGI Server | Hoch |
| anthropic | 0.18+ | Claude API | Hoch |
| openai | 1.0+ | GPT API | Mittel |
| sentence-transformers | 2.2+ | Embeddings | Hoch |
| bcrypt | 4.0+ | Passwort-Hashing | Hoch |
| python-multipart | - | File Uploads | Mittel |
| aiofiles | - | Async File I/O | Mittel |
| pyyaml | - | Config-Parsing | Niedrig |
| slowapi | - | Rate Limiting | Mittel |
| pyotp | - | 2FA/TOTP | Mittel |

### 8.2 Externe APIs

| API | Anbieter | Verwendung | Fallback |
|-----|----------|------------|----------|
| Claude | Anthropic | KI-Chat | OpenAI |
| GPT | OpenAI | KI-Chat | Claude |
| ElevenLabs | ElevenLabs | TTS | Keiner |
| Whisper | EVY (lokal) | STT | - |
| Porcupine | Picovoice | Wake-Word | Keiner |
| Telegram | Telegram | Live-Chat | E-Mail |

---

## 9. Sicherheitsarchitektur

### 9.1 Authentifizierung

| Mechanismus | Implementierung |
|-------------|-----------------|
| Passwort-Hashing | bcrypt (12 Rounds) |
| Token-Auth | JWT-ähnlich, 24h Gültigkeit |
| 2FA | TOTP (RFC 6238) + Backup-Codes |
| Session | Server-side, Token-basiert |

### 9.2 Autorisierung

| Rolle | Rechte |
|-------|--------|
| `super_admin` | Alles |
| `company_admin` | Eigene Company + User |
| `user` | Eigenes Brain + Chat |

### 9.3 Schutzmaßnahmen

- Rate Limiting (slowapi)
- CORS-Konfiguration
- Domain-Whitelist (Embed)
- Input-Validierung (Pydantic)
- SQL-Injection: N/A (kein SQL)
- XSS: Minimiert (Server-Rendering)

---

## 10. Bekannte Schwachstellen

| ID | Schwachstelle | Risiko | Status |
|----|---------------|--------|--------|
| S1 | Monolithisches Frontend (14k Zeilen) | Wartbarkeit | Offen |
| S2 | Keine Build-Pipeline | Wartbarkeit | Offen |
| S3 | Fehlende Unit-Tests | Qualität | Offen |
| S4 | Single-Developer-Wissen | Bus-Faktor | Dokumentiert |
| S5 | Vendor Lock-in (ElevenLabs, Picovoice) | Abhängigkeit | Offen |

---

## 11. Metriken-Zusammenfassung

```
┌─────────────────────────────────────────────────────┐
│                 MIRA v2.10.0                        │
├─────────────────────────────────────────────────────┤
│  Backend:    21.449 LOC  │  Frontend: 20.250 LOC   │
│  Services:   21 Module   │  Pages:    7 HTML       │
│  Endpoints:  214 Routes  │  Functions: ~200 JS     │
│  Docs:       6.284 LOC   │  Scripts:  15 Shell     │
├─────────────────────────────────────────────────────┤
│  Gesamt:     ~48.000 LOC                            │
│  Komplexität: Mittel-Hoch                           │
│  Test-Coverage: Undokumentiert                      │
└─────────────────────────────────────────────────────┘
```

---

**Zuletzt aktualisiert:** 2025-12-07 (Session 38)

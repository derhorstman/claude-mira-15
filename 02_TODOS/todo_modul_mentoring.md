# Todo: Mentoring-Modul für MIRA Companies

**Status:** Geplant

**Ziel:** Companies können eigene Mentoring-Programme für Mitarbeiter betreiben - mit KI-gestützter Reflexion und Fortschrittsverfolgung.

**Basis:** EVY Mentorisierer (`/opt/EVY/04_mentorisierer/mentorisierer.py`)

---

## Das Konzept

### Was ist Mentoring hier?

- Mitarbeiter reflektieren regelmäßig (täglich/wöchentlich)
- Per Text oder Sprache
- KI analysiert, gibt Feedback, erkennt Muster
- Fortschritt wird über Zeit sichtbar
- Company-Mentor (Mensch) kann Einblick nehmen

### Use-Cases

| Branche | Anwendung |
|---------|-----------|
| **Pflege** | Reflexion nach schwierigen Situationen |
| **Vertrieb** | Kundengespräche nachbereiten |
| **Azubis** | Ausbildungsbegleitung, Berichtsheft-Ersatz |
| **Führungskräfte** | Selbstreflexion, Coaching-Begleitung |
| **Teams** | Retrospektiven, Stimmungsbarometer |

---

## Features

### Für Mitarbeiter (Mentees)

| Feature | Beschreibung |
|---------|--------------|
| **Reflexions-Eingabe** | Text oder Sprache |
| **Strukturierte Prompts** | "Was lief gut?", "Was war schwierig?", "Was nehme ich mit?" |
| **KI-Feedback** | Analyse, Muster erkennen, Impulse geben |
| **History** | Eigene Reflexionen über Zeit sehen |
| **Privat-Modus** | Einträge die nur der Mitarbeiter sieht |

### Für Company-Mentor (Admin)

| Feature | Beschreibung |
|---------|--------------|
| **Dashboard** | Übersicht aller Mentees |
| **Aktivität** | Wer reflektiert regelmäßig, wer nicht? |
| **Themen-Trends** | Welche Themen kommen oft vor? |
| **Einzelansicht** | Fortschritt eines Mentees (wenn freigegeben) |
| **Interventions-Hinweise** | "Mitarbeiter X hat 3x Überlastung erwähnt" |

### Für Superadmin

| Feature | Beschreibung |
|---------|--------------|
| **Modul aktivieren** | Pro Company an/aus |
| **Limits** | Max. Mentees pro Company |
| **Anonyme Statistik** | Nutzung über alle Companies |

---

## Technische Umsetzung

### Datenstruktur

```
/opt/MIRA/companies/[company]/
├── mentoring/
│   ├── config.json           # Modul-Einstellungen
│   ├── mentees/
│   │   ├── [user1]/
│   │   │   ├── reflections/
│   │   │   │   ├── 2025-12-07_0930.json
│   │   │   │   └── 2025-12-08_1400.json
│   │   │   ├── analysis/     # KI-Analysen
│   │   │   └── summary.json  # Zusammenfassung, Trends
│   │   └── [user2]/
│   └── templates/            # Reflexions-Vorlagen
```

### Reflexions-Eintrag (JSON)

```json
{
  "id": "ref-20251207093012",
  "user": "mueller",
  "timestamp": "2025-12-07T09:30:12",
  "type": "daily",
  "input_mode": "voice",
  "content": {
    "raw_text": "Heute war ein schwieriger Tag...",
    "structured": {
      "gut": "Kundengespräch lief super",
      "schwierig": "Zeitdruck bei Projekt X",
      "mitnehmen": "Früher eskalieren"
    }
  },
  "ai_analysis": {
    "stimmung": "gemischt",
    "themen": ["zeitdruck", "kunde", "lernen"],
    "feedback": "Du erkennst selbst, dass frühere Eskalation hilft...",
    "muster": "Zeitdruck wurde in 3 der letzten 5 Einträge erwähnt"
  },
  "visibility": "mentor",  # private | mentor | team
  "audio_ref": null
}
```

### Neue Backend-Komponenten

| Datei | Beschreibung |
|-------|--------------|
| `mira_mentoring.py` | Mentoring-Service |
| `gateway.py` | Endpoints `/api/mentoring/*` |

### API-Endpoints

```
POST /api/mentoring/reflect          # Neue Reflexion einreichen
GET  /api/mentoring/history          # Eigene History
GET  /api/mentoring/analysis         # KI-Analyse abrufen
GET  /api/mentoring/dashboard        # Mentor-Dashboard (Admin)
GET  /api/mentoring/mentee/{user}    # Einzelner Mentee (Admin)
POST /api/mentoring/template         # Vorlage erstellen (Admin)
```

### Frontend

- **Reflexions-Widget** - Kann im Chat oder als eigene Seite sein
- **Mentor-Dashboard** - Neue Seite für Company-Admins
- **History-Ansicht** - Timeline der eigenen Reflexionen

---

## Von EVY übernehmen

### Was passt

| EVY-Feature | MIRA-Adaption |
|-------------|---------------|
| File-Watcher | → API-Endpoint (kein Dateisystem-Watch nötig) |
| User-Erkennung | → Aus Auth-Token |
| Brain-Speicherung | → Company-spezifisches Brain |
| History (JSON) | → Gleiche Struktur |
| Chat-Analyse | → MIRA Chat-API nutzen |

### Was neu ist

- Multi-Tenant (Company-Isolation)
- Mentor-Dashboard
- Sichtbarkeits-Steuerung (privat/mentor/team)
- Strukturierte Reflexions-Vorlagen
- Trend-Erkennung über Zeit

---

## Reflexions-Vorlagen

### Tägliche Reflexion (Standard)

```
1. Was lief heute gut?
2. Was war herausfordernd?
3. Was habe ich gelernt?
4. Was nehme ich mir für morgen vor?
```

### Wöchentliche Reflexion

```
1. Highlights der Woche
2. Größte Herausforderung
3. Was hat mich überrascht?
4. Feedback das ich bekommen habe
5. Ziele für nächste Woche
```

### Nach schwieriger Situation (Pflege/Sozial)

```
1. Was ist passiert? (Fakten)
2. Wie habe ich mich gefühlt?
3. Was habe ich getan?
4. Was hätte ich anders machen können?
5. Was brauche ich jetzt?
```

### Kundengespräch (Vertrieb)

```
1. Kunde / Situation
2. Was war das Ziel?
3. Wie lief das Gespräch?
4. Was hat funktioniert?
5. Was mache ich beim nächsten Mal anders?
```

---

## Datenschutz

### Grundsätze

- Reflexionen gehören dem Mitarbeiter
- Sichtbarkeit wird vom Mitarbeiter gesteuert
- Company-Admin sieht nur freigegebene Inhalte
- Superadmin sieht keine Inhalte (nur Statistiken)

### Sichtbarkeits-Stufen

| Stufe | Wer sieht es |
|-------|--------------|
| `private` | Nur der Mitarbeiter selbst |
| `mentor` | Mitarbeiter + zugewiesener Mentor |
| `team` | Mitarbeiter + Team (anonymisiert möglich) |

### Löschung

- Mitarbeiter kann eigene Daten jederzeit löschen
- Bei Austritt: Automatische Löschung nach X Tagen

---

## Lizenzierung

| Plan | Beschreibung | Preis-Idee |
|------|--------------|------------|
| **Mentoring Basic** | 10 Mentees, Standard-Vorlagen | 39€/Monat |
| **Mentoring Pro** | 50 Mentees, Custom-Vorlagen, Dashboard | 79€/Monat |
| **Mentoring Enterprise** | Unbegrenzt, Trend-Analyse, API | Auf Anfrage |

---

## Synergien mit anderen Modulen

| Modul | Synergie |
|-------|----------|
| **Pflege** | Reflexion nach Schicht, Supervision-Ersatz |
| **Voice** | Reflexion per Sprache einsprechen |
| **Brain** | Reflexionen fließen ins persönliche Brain |

---

## Offene Fragen

- [ ] Braucht es einen menschlichen Mentor oder reicht KI?
- [ ] Gamification? (Streaks, Badges)
- [ ] Integration mit HR-Systemen?
- [ ] Mobile App sinnvoll?

---

## Nächste Schritte

1. [ ] Use-Case mit bestehendem Kunden validieren
2. [ ] MVP: Reflexions-Eingabe + KI-Feedback
3. [ ] Mentor-Dashboard
4. [ ] Vorlagen-System
5. [ ] Trend-Analyse

---

**Zuletzt aktualisiert:** 2025-12-07

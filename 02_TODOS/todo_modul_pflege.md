# Todo: Pflege-Dokumentationsmodul

**Status:** Geplant

**Ziel:** MIRA-Modul für Pflegedienste - Dokumentationshilfe per Sprache, DSGVO-konform.

---

## Das Problem

- Pflegekräfte dokumentieren viel (Übergaben, Maßnahmen, Berichte, Wundprotokolle)
- Zeitdruck, oft unterwegs am Handy
- Aktuelle Lösung: Tippen oder nachträglich am PC
- Datenschutz-Angst: Patientendaten in der Cloud?

---

## Die Lösung: MIRA Pflege-Modul

### Kern-Features

| Feature | Beschreibung |
|---------|--------------|
| **Sprache → Doku** | Pflegekraft spricht, MIRA macht strukturierten Text |
| **Vorlagen** | Übergabe, Tagesbericht, Wunddoku, Vitalwerte |
| **Pseudonymisierung** | Klarnamen werden vor KI-Call ersetzt |
| **Lokale STT** | Whisper auf EVY - Sprache verlässt nicht den Server |
| **Audit-Log** | Wer hat wann was dokumentiert |
| **Export** | PDF, Schnittstelle zu Pflegesoftware (später) |

### Beispiel-Workflow

1. Pflegekraft öffnet MIRA App / Widget
2. Drückt Mikrofon oder sagt "Hey MIRA"
3. Spricht: "Frau Schmidt, Zimmer 12, Blutdruck 145/90, Medikament Ramipril gegeben, Patientin war unruhig"
4. MIRA erkennt:
   - Patient: [Pseudonym P-012]
   - Vitalwerte: RR 145/90
   - Medikament: Ramipril
   - Bemerkung: Unruhe
5. Generiert strukturierten Eintrag
6. Pflegekraft bestätigt → gespeichert

---

## Datenschutz-Konzept

### Rechtliche Grundlage

| Aspekt | Umsetzung |
|--------|-----------|
| **DSGVO Art. 9** | Gesundheitsdaten = besondere Kategorie |
| **Rechtsgrundlage** | Art. 6 Abs. 1 lit. b (Vertrag) oder Art. 9 Abs. 2 lit. h (Gesundheitsversorgung) |
| **AVV** | Auftragsverarbeitungsvertrag mit Pflegedienst |
| **Verantwortlicher** | Der Pflegedienst, nicht wir |
| **Auftragsverarbeiter** | Systemhaus Horst (MIRA) |

### Technische Maßnahmen (TOM)

| Maßnahme | Beschreibung |
|----------|--------------|
| **Pseudonymisierung** | Patientennamen → Codes vor KI-Verarbeitung |
| **Verschlüsselung** | TLS in Transit, verschlüsselt at Rest |
| **Lokale STT** | Whisper auf EVY (192.168.42.15) - keine externe API |
| **Datenminimierung** | Nur nötige Daten an LLM |
| **Zugriffskontrolle** | Nur autorisierte Mitarbeiter |
| **Audit-Log** | Alle Zugriffe protokolliert |
| **Löschkonzept** | Aufbewahrungsfristen, danach Löschung |
| **Serverstandort** | Deutschland (dein Server) |

### Pseudonymisierung im Detail

```
Eingabe (Sprache):
"Frau Schmidt in Zimmer 12 hat Fieber"

Nach Pseudonymisierung (an KI):
"Patient P-012 hat Fieber"

Rückübersetzung (für Ausgabe):
"Frau Schmidt (Zimmer 12) hat Fieber"
```

**Mapping-Tabelle:** Bleibt lokal, geht nie an externe API.

---

## Technische Umsetzung

### Neue Komponenten

| Komponente | Beschreibung |
|------------|--------------|
| `mira_pflege.py` | Pflege-Modul Backend |
| `pflege_vorlagen/` | JSON-Vorlagen für Doku-Typen |
| `pflege_mapping.json` | Patient-Pseudonym Zuordnung (pro Company) |
| Pflege-UI | Spezielles Interface für Dokumentation |

### Datenstruktur

```
/opt/MIRA/companies/[pflegedienst]/
├── pflege/
│   ├── mapping.json          # Patient → Pseudonym
│   ├── vorlagen/             # Doku-Vorlagen
│   ├── dokumentation/        # Gespeicherte Einträge
│   │   ├── 2025-12-07/
│   │   │   ├── P-012_0930.json
│   │   │   └── P-015_1045.json
│   └── audit.log             # Zugriffs-Protokoll
```

### Beispiel Doku-Eintrag

```json
{
  "id": "dok-2025120709301234",
  "patient_pseudo": "P-012",
  "timestamp": "2025-12-07T09:30:00",
  "erstellt_von": "pfleger_mueller",
  "typ": "tagesbericht",
  "inhalt": {
    "vitalwerte": {
      "blutdruck": "145/90",
      "puls": 78,
      "temperatur": null
    },
    "medikamente": ["Ramipril 5mg"],
    "bemerkung": "Patientin war unruhig",
    "massnahmen": []
  },
  "original_audio": null,  # Optional: Audio-Referenz
  "freigabe": true
}
```

---

## Vorlagen

### Übergabe

```
Schicht: [Früh/Spät/Nacht]
Datum: [Datum]
Besonderheiten:
- Patient [X]: [Beschreibung]
- Patient [Y]: [Beschreibung]
Offene Aufgaben:
- [ ] [Aufgabe]
```

### Vitalwerte

```
Patient: [Pseudonym]
Zeitpunkt: [Uhrzeit]
- Blutdruck: [sys/dia]
- Puls: [bpm]
- Temperatur: [°C]
- Blutzucker: [mg/dl]
- Sauerstoff: [%]
Bemerkung: [Text]
```

### Wunddokumentation

```
Patient: [Pseudonym]
Lokalisation: [Körperstelle]
Größe: [cm x cm]
Tiefe: [mm]
Wundgrund: [Granulation/Fibrin/Nekrose]
Exsudat: [wenig/mittel/viel]
Wundrand: [intakt/mazeriert]
Geruch: [ja/nein]
Maßnahme: [Verband/Spülung/etc.]
Nächster VW: [Datum]
Foto: [ja/nein]
```

---

## Lizenzierung

### Als MIRA-Modul

| Plan | Beschreibung | Preis (Idee) |
|------|--------------|--------------|
| **Pflege Basis** | 5 Mitarbeiter, STT+Doku | 49€/Monat |
| **Pflege Pro** | 20 Mitarbeiter, + Vorlagen + Export | 99€/Monat |
| **Pflege Enterprise** | Unbegrenzt, + Schnittstellen | Auf Anfrage |

### Zusatz-Module

- Wunddokumentation mit Foto
- Schnittstelle zu Pflegesoftware (Connext Vivendi, Medifox, etc.)
- Offline-Modus (später)

---

## Vertriebsargumente

### Für Pflegedienstleitung

> "Ihre Mitarbeiter dokumentieren per Sprache - in Sekunden statt Minuten. DSGVO-konform, Daten bleiben bei Ihnen."

### Gegen Datenschutz-Bedenken

> "Patientennamen werden pseudonymisiert bevor die KI sie sieht. Die Spracherkennung läuft auf unserem deutschen Server, nicht bei Google oder Amazon."

### ROI

> "10 Pflegekräfte × 30 Min Doku-Ersparnis/Tag × 20 Tage = 100 Stunden/Monat. Was kostet eine Pflegekraft-Stunde?"

---

## Offene Fragen

- [ ] Welche Pflegesoftware nutzen deine Kunden? (Schnittstellen)
- [ ] Brauchen wir Zertifizierung? (ISO 27001, etc.)
- [ ] Mobile App oder nur Web?
- [ ] Offline-Fähigkeit nötig?

---

## Nächste Schritte

1. [ ] Mit Pflegedienst-Kunden sprechen: Was genau brauchen sie?
2. [ ] Muster-AVV erstellen
3. [ ] Pseudonymisierungs-Logik bauen
4. [ ] Vorlagen mit Pflegekraft validieren
5. [ ] MVP bauen (nur Sprache → Text)
6. [ ] Testen mit einem Pflegedienst

---

**Zuletzt aktualisiert:** 2025-12-07

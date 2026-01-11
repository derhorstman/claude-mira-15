# Todo: Abrechnungssystem

**Status:** In Arbeit (MiraCoin-Basis fertig)

**Ziel:** Vollständiges Abrechnungssystem mit automatischer Partner-Provision.

---

## Bereits implementiert ✅

### MiraCoin System (Session 32)

- [x] **Wallet-System** für Partner
  - Jeder Partner hat ein Wallet mit Balance
  - Transaktionshistorie (Provision, Bonus, Auszahlung, Korrektur)
  - 1 MiraCoin = 1 EUR

- [x] **Auszahlungssystem**
  - Bankdaten hinterlegen (IBAN, BIC, Kontoinhaber)
  - Auszahlung beantragen (ab 50 MC)
  - Superadmin: Genehmigen/Ablehnen/Als bezahlt markieren

- [x] **Frontend**
  - 🪙 MiraCoins Navigation für Company-Admins
  - Wallet-Seite mit Stats, Transaktionen, Auszahlung
  - Superadmin: Übersicht aller Wallets, Bonus vergeben

- [x] **API Endpoints**
  - `/api/coins/wallet` - Eigenes Wallet
  - `/api/coins/transactions` - Transaktionen
  - `/api/coins/payout-info` - Bankdaten
  - `/api/coins/payout` - Auszahlung beantragen
  - `/api/superadmin/coins/*` - Admin-Funktionen

---

## Noch zu implementieren

### Phase 1: Partner-Tracking

- [ ] **referred_by Feld bei Company**
  - Welcher Partner hat die Company geworben?
  - Bei Demo-Erstellung automatisch setzen
  - Nachträglich zuweisbar durch Superadmin

- [ ] **Automatische Provision bei Zahlung**
  - Wenn Company zahlt → Partner bekommt 20% als MiraCoins
  - Trigger: Rechnung als "bezahlt" markieren
  - Rückbuchung bei Kündigung/Storno

### Phase 2: Company-Abrechnung

- [ ] **Preismodelle pro Company**
  ```json
  {
    "plan": "starter|business|enterprise",
    "base_fee": 49.00,
    "token_price": 0.002,
    "included_tokens": 100000,
    "billing_cycle": "monthly",
    "payment_terms_days": 14
  }
  ```

- [ ] **Rechnungserstellung**
  - Automatisch zum Monatsende
  - Basierend auf Token-Verbrauch (vorhanden!)
  - PDF-Generierung (wkhtmltopdf vorhanden)

- [ ] **Rechnungsversand**
  - Per E-Mail (SMTP vorhanden)
  - Im Serviceportal abrufbar (vorhanden!)

### Phase 3: Zahlungseingang

- [ ] **Zahlung markieren**
  - Superadmin markiert Rechnung als bezahlt
  - → Trigger: Partner-Provision gutschreiben

- [ ] **Automatischer Zahlungseingang** (optional)
  - Stripe/PayPal Integration
  - SEPA-Lastschrift
  - Webhook bei Zahlung → Provision automatisch

### Phase 4: Reporting

- [ ] **Superadmin Dashboard**
  - Offene Rechnungen gesamt
  - Erwartete Einnahmen
  - Partner-Provisionen ausstehend

- [ ] **Company Dashboard**
  - Aktuelle Rechnung
  - Zahlungshistorie
  - Token-Verbrauch vs. Limit

---

## Technische Umsetzung

### Neue/Geänderte Dateien

| Datei | Status | Beschreibung |
|-------|--------|--------------|
| `mira_coins.py` | ✅ Fertig | MiraCoin/Wallet System |
| `mira_billing.py` | 🔲 Offen | Abrechnungslogik |
| `mira_invoice.py` | 🔲 Offen | PDF-Generierung |
| `mira_company.py` | 🔲 Erweitern | referred_by, billing Felder |
| `gateway.py` | ✅ Coins fertig | Billing Endpoints fehlen |
| `index.html` | ✅ Wallet fertig | Billing UI fehlt |

### Datenstruktur

```
/opt/MIRA/data/coins/
├── wallets.json        ✅ Fertig
├── transactions.json   ✅ Fertig
└── payouts.json        ✅ Fertig

/opt/MIRA/companies/[slug]/
├── billing.json        🔲 Neu - Preismodell
└── documents/
    └── invoices/       ✅ Vorhanden (Serviceportal)
```

### Abhängigkeiten

- [x] Token-Tracking (usage.json pro Company)
- [x] E-Mail-Versand (mira_mail.py)
- [x] PDF-Generierung (wkhtmltopdf)
- [x] Serviceportal für Rechnungen
- [x] MiraCoin Wallet-System

---

## Nächste Schritte

1. **referred_by bei Company** hinzufügen
2. **mira_billing.py** erstellen (Preismodelle, Rechnungslogik)
3. **Automatische Provision** bei Zahlungsmarkierung
4. **Rechnungs-PDF** Template erstellen
5. **Billing-UI** im Superadmin

---

## Offene Fragen

- [ ] Welche Preismodelle genau? (Starter 49€, Business 99€, Enterprise 199€?)
- [ ] Zahlungsziel? (14 Tage?)
- [ ] Zahlungsanbieter? (Erstmal nur Rechnung, später Stripe?)
- [ ] Mahnwesen nötig?
- [ ] DATEV-Export?

---

**Zuletzt aktualisiert:** 2025-12-07 (MiraCoin System implementiert)

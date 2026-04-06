## Enkel import til Fiken for bilag 2025

### Kort konklusjon

Det finnes ikke en enkel standardimport i Fiken for generelle kostnadsbilag via CSV i brukergrensesnittet.

Den enkleste realistiske importveien for alle bilagene i 2025 er derfor:

1. lage et strukturert dokument i CSV-format
2. oversette CSV-radene til Fiken sitt `purchases`-API
3. laste opp vedlegg separat og knytte dem til hvert kjop

For bilag som ikke passer som vanlige kjop, kan vi eventuelt bruke `generalJournalEntries`, men det bor bare brukes for unntak. For hovedmengden er `purchases` enklest.

### Hva Fiken faktisk stotter

Basert pa Fiken-dokumentasjonen:

- vanlig fakturaimport i UI gjelder salgsfakturaer fra utvalgte systemer
- registrering av kjop og utgifter i UI er i hovedsak manuell
- API-et har egne endepunkter for:
  - `POST /companies/{companySlug}/purchases`
  - `POST /companies/{companySlug}/purchases/drafts`
  - `POST /companies/{companySlug}/generalJournalEntries`

### Anbefalt modell

Vi lager ett CSV-dokument der **en rad = en konteringslinje**.

Flere rader kan hore til samme bilag ved a bruke samme `group_id`.

Dette gir oss:

- enkel manuell kvalitetssikring i regneark
- enkel produksjon med AI eller scripts
- stotte for splitting av ett bilag over flere kontoer
- enkel mapping til Fiken sitt `purchaseRequest`

### Foreslatt CSV-mal

Se `fiken_purchases_template.csv`.

Viktigste kolonner:

- `group_id`: unik id for bilaget, samme verdi for alle linjer i samme bilag
- `line_no`: linjenummer innen bilaget
- `date`: fakturadato/kjopsdato, format `YYYY-MM-DD`
- `due_date`: forfallsdato, valgfritt
- `kind`: `supplier` eller `cash_purchase`
- `identifier`: fakturanummer eller annen referanse
- `supplier_name`: leverandornavn
- `supplier_org_number`: organisasjonsnummer hvis kjent
- `currency`: normalt `NOK`
- `paid`: `true` eller `false`
- `payment_account`: f.eks. `1920:10001` hvis betalt fra bank
- `payment_date`: betalingsdato hvis betalt
- `payment_amount_nok`: faktisk betalt NOK-belop ved utenlandsk valuta
- `account`: kostnadskonto, f.eks. `6540`
- `description`: beskrivelse av linjen
- `net_amount`: netto belop i kroner
- `vat_amount`: mva-belop i kroner
- `vat_type`: Fiken VAT type, f.eks. `HIGH`, `NONE`, `HIGH_FOREIGN_SERVICE_DEDUCTIBLE`
- `attachment_path`: filnavn eller sti til bilaget
- `kid`: valgfritt
- `project_id`: valgfritt
- `notes`: interne notater

### Mapping til Fiken API

Importer mapper hver `group_id` til ett `purchaseRequest`.

Eksempelstruktur:

```json
{
  "date": "2025-02-14",
  "dueDate": "2025-02-28",
  "kind": "supplier",
  "identifier": "INV-2025-001",
  "supplierId": 12345,
  "currency": "NOK",
  "paymentAccount": "1920:10001",
  "paymentDate": "2025-02-14",
  "lines": [
    {
      "description": "Cursor abonnement",
      "account": "6540",
      "vatType": "HIGH_FOREIGN_SERVICE_DEDUCTIBLE",
      "netPrice": 1600,
      "vat": 0
    }
  ]
}
```

Merk:

- Fiken API bruker belop i **ore**, ikke kroner
- `supplierId` ma finnes i Fiken; importer ma derfor finne eller opprette leverandor forst
- vedlegg lastes normalt opp separat og knyttes til kjopet etter opprettelse

### Nar vi bor bruke `purchases`

Bruk `purchases` for:

- leverandorfakturaer
- kvitteringer
- kortkjop
- abonnementer
- vanlige driftskostnader

Dette dekker sannsynligvis mesteparten av bilagene for 2025.

### Nar vi bor bruke `generalJournalEntries`

Bruk `generalJournalEntries` bare for spesialtilfeller som:

- manuelle omposteringer
- korrigeringer
- apningsposter
- bilag som ikke passer inn i vanlig kjopsflyt

Det er mer fleksibelt, men ogsa mer teknisk og lettere a fore feil med.

### Praktisk anbefaling for 2025

Den enkleste arbeidsflyten er:

1. samle alle bilag og transaksjoner for 2025
2. fylle ut CSV-malen
3. kontrollere konto, mva og betalingsstatus
4. importere radene via et lite script mot Fiken API
5. legge usikre bilag i en egen unntaksliste

### Hva jeg anbefaler at vi gjor videre

Neste naturlige steg er a lage:

1. det faktiske 2025-dokumentet i denne CSV-strukturen
2. et lite importscript som:
   - leser CSV
   - oppretter/finner leverandorer
   - oppretter kjop i Fiken
   - knytter vedlegg til riktig kjop

### Viktig avgrensning

Selv om importveien er enkel sammenlignet med manuell foring, ma mva-behandling og konto fortsatt kvalitetssikres for:

- utenlandske tjenester
- programvareabonnementer
- private andeler
- bilag med flere satser eller flere kontoer
- mangelfulle kvitteringer

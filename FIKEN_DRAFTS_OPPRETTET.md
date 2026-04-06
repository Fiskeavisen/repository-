## Fiken-utkast er opprettet

Det er nå opprettet **kjøpsutkast i Fiken** for bilagene vi har samlet.

### Hva som ble gjort

- leverandørkontakt for `Cursor` ble opprettet i Fiken
- leverandørkontakt for `Anthropic Claude` ble opprettet i Fiken
- kjøpene ble opprettet som **kjøpsutkast**
- de ble opprettet som **ubetalte**

Dette er med vilje, siden du har avklart at alt er **privat utlegg**.

Da kan du åpne hvert utkast i Fiken og velge:

- `Betalt privat / personlig utlegg`

ved godkjenning.

### Oppsummert resultat

- **60 kjøpsutkast opprettet**
- **0 hoppet over**

Se filer:

- `fiken_created_purchase_drafts.csv`
- `fiken_skipped_purchase_drafts.csv`

### Viktig

Selv om noen rader tidligere var merket som avvik eller ventende kortmatch, er de nå likevel opprettet som utkast fordi du ba om at fakturaene bare skulle inn, og at det er greit at du godkjenner etterpå.

Det betyr at du fortsatt bor se ekstra pa:

- refund-radene
- sene desemberrader
- eventuelle rader der betalingsmatch ikke var komplett

### Hva du gjor i Fiken na

1. Gå til `Kjøp`
2. Åpne kjøpsutkastene
3. Kontroller hvert utkast
4. Velg betalingsmåte:
   - `Betalt privat / personlig utlegg`
5. Registrer kjøpet

### Teknisk detalj

Utkastene ble opprettet med:

- valuta `USD`
- kostnadskonto `6420 - Leie datasystemer`
- MVA-type `NONE`

Hvis du vil endre konto eller MVA-behandling på enkelte rader, gjør du det i utkastet før registrering.

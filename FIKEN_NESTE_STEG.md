## Hva du gjor na for at Fiken skal fa dette

### Kort status

Det finnes ikke en enkel standard CSV-import i Fiken for generelle kostnadsbilag i brukergrensesnittet.

Den praktiske veien videre er derfor:

1. bruke datagrunnlaget vi har laget
2. avklare hvordan betalingene skal fores i Fiken
3. enten:
   - importere via Fiken API, eller
   - registrere kjopene manuelt med dette som fasit

### Hva som allerede er klart

Av bilagene vi har jobbet med sa langt har vi:

- **53 eksakte kortmatcher** som er gode kandidater for foring na
- samlet kjent belop:
  - **4296.59 USD**
  - **44531.88 NOK**

Relevante filer:

- `cursor_2025_billing_reconciled.csv`
- `claude_2025_billing_reconciled.csv`
- `fiken_2025_open_items.csv`

### Det viktigste valget du ma ta i Fiken

For at jeg skal kunne gjore dette riktig i Fiken ma vi vite **hvordan betalingene skal behandles**:

#### Alternativ A - dette er betalt av foretakets konto/kort

Da trenger jeg:

- hvilken **betalingskonto i Fiken** som skal brukes
- typisk en konto som `1920:...` hvis dette er en registrert bankkonto i Fiken

#### Alternativ B - dette er betalt privat og skal fores som utlegg

Da skal kjopene normalt **ikke** bokfores som betalt fra foretakets bankkonto.
De bor i stedet fores som **utlegg / privat betalt kostnad**, avhengig av hvordan du forer dette i Fiken.

Dette er det viktigste avklaringspunktet for at importen skal bli riktig.

### Det du kan gjore na

#### Hvis du vil at jeg skal gjore bulkimport til Fiken

Gjor dette:

1. Aktiver **Fiken API** i Fiken under:
   - `Foretak -> Tilleggstjenester`
2. Opprett en **personlig API-nokkel**
3. Finn eller bekreft:
   - **company slug** i Fiken
   - om disse skal fores som:
     - betalt fra foretakets konto, eller
     - private utlegg
   - hvis foretakets konto: hvilken **betalingskonto** i Fiken som skal brukes
4. Last opp neste kortfaktura for:
   - sene desember-kjop
   - eventuelt eldre januar 2025-kjop
5. Se gjennom `fiken_2025_open_items.csv`

Nar dette er avklart kan jeg lage:

- en endelig Fiken-klargjort importfil
- og/eller et importscript mot Fiken API

#### Hvis du vil legge inn manuelt i Fiken

Da er arbeidsflyten:

1. Aapne `cursor_2025_billing_reconciled.csv`
2. Aapne `claude_2025_billing_reconciled.csv`
3. For alle rader med `exact_amount_match`:
   - registrer kjopet i Fiken
   - bruk USD-belopet som kjopsbelop
   - bruk kjent NOK-belop som kontroll mot betaling
   - legg ved faktura / dokumentasjon hvis du har den
4. For alle rader i `fiken_2025_open_items.csv`:
   - vent til vi har mer data
   - eller avklar refusjon / neste kortfaktura forst

### Min anbefaling

Den tryggeste og raskeste veien videre er:

1. du svarer pa dette ene sporsmalet:
   - **Er disse betalingene gjort privat eller fra foretakets egen konto/kort?**
2. du laster opp neste kortfaktura
3. jeg lager den endelige Fiken-klare fila for det som er sikkert

Hvis du vil, kan neste steg vaere at jeg lager en **ferdig Fiken-importdraft** for alle de 53 sikre radene, basert pa at du forteller meg om dette er:

- `utlegg`, eller
- `foretakets konto`

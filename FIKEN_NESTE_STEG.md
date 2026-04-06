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

### Betalingsbehandling er na avklart

Du har avklart at **alt er privat utlegg**.

Det betyr:

- kjopene skal **ikke** bokfores som betalt fra foretakets bankkonto
- de bor fores som **utlegg / privat betalt kostnad**
- kjent NOK-belop brukes som kontroll mot det du faktisk har lagt ut privat

### Det du kan gjore na

#### Hvis du vil at jeg skal gjore bulkimport til Fiken

Gjor dette:

1. Aktiver **Fiken API** i Fiken under:
   - `Foretak -> Tilleggstjenester`
2. Opprett en **personlig API-nokkel**
3. Finn eller bekreft:
   - **company slug** i Fiken
4. Last opp neste kortfaktura for:
   - sene desember-kjop
   - eventuelt eldre januar 2025-kjop
5. Se gjennom `fiken_2025_open_items.csv`
6. Bruk `fiken_2025_private_utlegg_ready.csv` som grunnlag for det som allerede er klart

Nar dette er avklart kan jeg lage:

- en endelig Fiken-klargjort importfil
- og/eller et importscript mot Fiken API

#### Hvis du vil legge inn manuelt i Fiken

Da er arbeidsflyten:

1. Aapne `cursor_2025_billing_reconciled.csv`
2. Aapne `claude_2025_billing_reconciled.csv`
3. Aapne `fiken_2025_private_utlegg_ready.csv`
4. For hver rad i den fila:
   - registrer kjopet i Fiken
   - for betalingen: velg **utlegg / privat betalt**
   - bruk USD-belopet som leverandorbelop
   - bruk kjent NOK-belop som kontroll mot hva du faktisk la ut privat
   - legg ved faktura / dokumentasjon hvis du har den
5. For alle rader i `fiken_2025_open_items.csv`:
   - vent til vi har mer data
   - eller avklar refusjon / neste kortfaktura forst

### Min anbefaling

Den tryggeste og raskeste veien videre er:

1. bruk `fiken_2025_private_utlegg_ready.csv` for alle sikre rader
2. last opp neste kortfaktura
3. jeg oppdaterer avvikene og lager neste ferdige batch

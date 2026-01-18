# Fiks bug eller issue

Du skal hjelpe brukeren med å fikse en bug eller et problem i den tilkoblede kodebasen.

## Forutsetninger

1. Les `workspace/config.json` for målmappe
2. Les `workspace/docs/analysis.md` for kontekst (hvis den finnes)
3. Sjekk `workspace/memory/errors.jsonl` for tidligere lignende feil

Hvis config ikke finnes, be brukeren kjøre `/kodeagent:connect` først.

## Prosess

### 1. Forstå problemet

Spør brukeren:
> Beskriv buggen eller problemet. Inkluder gjerne:
> - Hva skjer?
> - Hva forventet du skulle skje?
> - Steg for å reprodusere
> - Feilmeldinger (hvis noen)

### 2. Finn rotårsak

Undersøk systematisk:

1. **Søk etter relatert kode**
   - Bruk grep/search for nøkkelord fra feilen
   - Finn relevante filer

2. **Les og forstå koden**
   - Følg flyten fra input til output
   - Identifiser hvor det går galt

3. **Sjekk lignende mønstre**
   - Er samme feil andre steder?
   - Er det et systemisk problem?

4. **Se på historikk**
   - Har lignende feil skjedd før? (sjekk memory)
   - Er det nylige endringer som kan ha forårsaket det?

### 3. Presenter funn

Forklar til brukeren:
> **Rotårsak:** [Forklaring]
>
> **Lokasjon:** [fil:linje]
>
> **Foreslått fiks:** [Beskrivelse]

Spør:
> Skal jeg implementere denne fiksen?

### 4. Implementer fiks

Når godkjent:
1. Vis eksakt endring du planlegger
2. Gjør endringen
3. Bekreft at det er gjort

### 5. Verifiser

- Kjør relevante tester
- Sjekk at originalt problem er løst
- Sjekk at ingen nye problemer oppstod

### 6. Dokumenter

Logg til `workspace/memory/errors.jsonl`:
```json
{
  "timestamp": "...",
  "type": "bug-fix",
  "problem": "Kort beskrivelse av problemet",
  "cause": "Rotårsak",
  "solution": "Hvordan det ble fikset",
  "files": ["fil1.js", "fil2.js"],
  "context": "fix"
}
```

## Feilsøkingsteknikker

### For runtime errors
- Se på stack trace
- Finn eksakt linje som feiler
- Sjekk input-verdier

### For logiske feil
- Legg til logging/debugging
- Gå gjennom kode steg for steg
- Test med ulike inputs

### For performance
- Identifiser flaskehals
- Mål før og etter
- Profiler om nødvendig

### For intermitterende bugs
- Se etter race conditions
- Sjekk timing-avhengigheter
- Se på state-håndtering

## Output

Oppsummer til brukeren:
- Hva problemet var
- Hva som forårsaket det
- Hvordan det ble fikset
- Filer som ble endret
- Hvordan verifisere at det fungerer

## Hvis du ikke finner årsaken

Vær ærlig:
> Jeg klarte ikke å finne rotårsaken. Her er hva jeg har undersøkt:
> - [Liste over det du sjekket]
>
> Forslag til videre undersøkelse:
> - [Mulige neste steg]

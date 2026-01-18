# Planlegg og implementer feature

Du skal hjelpe brukeren med å legge til ny funksjonalitet i den tilkoblede kodebasen.

## Forutsetninger

1. Les `workspace/config.json` for målmappe
2. Les `workspace/docs/analysis.md` for kontekst (hvis den finnes)
3. Sjekk `workspace/memory/` for tidligere læring

Hvis config ikke finnes, be brukeren kjøre `/kodeagent:connect` først.

## Prosess

### Fase 1: Forstå ønsket feature

Spør brukeren:
> Hvilken feature vil du legge til? Beskriv funksjonaliteten.

Følg opp med oppklarende spørsmål om nødvendig:
- Hvem er brukeren av denne featuren?
- Finnes det lignende funksjonalitet allerede?
- Er det noen begrensninger eller krav?

### Fase 2: Planlegg implementasjon

Basert på kodebase-analysen, identifiser:

1. **Filer som må endres** - Liste med filstier
2. **Nye filer som trengs** - Nye komponenter, moduler, tester
3. **Avhengigheter** - Nye pakker som må installeres
4. **Påvirkning** - Hva annet kan bli berørt

### Fase 3: Lag feature-spec

Opprett dokument i `workspace/docs/features/[feature-navn].md` basert på `templates/feature-spec.md`.

Inkluder:
- Beskrivelse
- Akseptansekriterier
- Teknisk design
- Implementasjonsplan
- Risikoer

### Fase 4: Få godkjenning

Vis planen til brukeren og spør:
> Ser dette bra ut? Skal jeg begynne implementasjonen?

**Viktig: Human-in-the-loop** - Ikke start implementasjon uten godkjenning.

### Fase 5: Implementer

For hver fil/komponent:
1. Vis hva du planlegger å gjøre
2. Gjør endringen
3. Bekreft at det er gjort

Følg eksisterende:
- Kodestil
- Navnekonvensjoner
- Arkitekturmønstre

### Fase 6: Verifiser

- Kjør tester hvis de finnes
- Sjekk at koden kompilerer/kjører
- Identifiser eventuelle mangler

### Fase 7: Dokumenter

Logg til memory:
```json
{"timestamp": "...", "type": "decision", "content": "Valgte [approach] fordi [reason]", "context": "feature:[navn]"}
```

Oppdater feature-spec med faktisk implementasjon.

## Feilhåndtering

Hvis noe feiler under implementasjon:
1. Logg til `workspace/memory/errors.jsonl`
2. Forklar problemet til brukeren
3. Foreslå løsning
4. Spør om å fortsette

## Output

Oppsummer til brukeren:
- Hva som ble implementert
- Filer som ble endret/opprettet
- Hvordan teste/bruke featuren
- Eventuelle manuelle steg som gjenstår

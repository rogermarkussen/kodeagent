# Kodegjennomgang

Du skal gjøre en grundig kodegjennomgang av den tilkoblede kodebasen.

## Forutsetninger

1. Les `workspace/config.json` for målmappe
2. Les `workspace/docs/analysis.md` for kontekst (hvis den finnes)

Hvis config ikke finnes, be brukeren kjøre `/kodeagent:connect` først.

## Gjennomgang

### 1. Spør om scope

Spør brukeren:
> Vil du gjennomgå hele kodebasen, eller fokusere på spesifikke deler?
>
> Alternativer:
> - Hele kodebasen
> - Spesifikk mappe (f.eks. src/api/)
> - Spesifikke filer
> - Nylige endringer (git diff)

### 2. Sjekk for bugs

Se etter:
- **Logiske feil** - Feil i if/else, loops, beregninger
- **Null/undefined** - Manglende sjekker
- **Edge cases** - Grensetilfeller som ikke håndteres
- **Race conditions** - Asynkrone problemer
- **Memory leaks** - Ressurser som ikke frigis

### 3. Sikkerhetsproblemer

Sjekk for:
- **Injection** - SQL, command, XSS
- **Autentisering** - Manglende eller svak auth
- **Autorisasjon** - Tilgangskontroll
- **Sensitive data** - Hardkodede secrets, logging av passord
- **Input validering** - Manglende sanitering

### 4. Kodekvalitet

Vurder:
- **Lesbarhet** - Er koden lett å forstå?
- **Vedlikeholdbarhet** - Er den enkel å endre?
- **DRY** - Er det duplisert kode?
- **SOLID** - Følges gode prinsipper?
- **Naming** - Er navngiving beskrivende?

### 5. Ytelse

Se etter:
- **N+1 queries** - Database-problemer
- **Unødvendige loops** - Ineffektive algoritmer
- **Store payloads** - Overflødig data
- **Manglende caching** - Gjentatte beregninger

### 6. Testing

Vurder:
- **Dekning** - Er kritisk kode testet?
- **Testkvalitet** - Er testene gode?
- **Manglende tester** - Hva burde testes?

## Generer rapport

Lag `workspace/docs/review-[dato].md` basert på `templates/code-review.md`.

### Prioritering

Kategoriser funn:

| Prioritet | Beskrivelse |
|-----------|-------------|
| 🔴 Kritisk | Må fikses umiddelbart (sikkerhet, data-tap) |
| 🟠 Høy | Bør fikses snart (bugs, ytelse) |
| 🟡 Medium | Bør fikses (kodekvalitet) |
| 🟢 Lav | Nice-to-have (forbedringer) |

## Output til bruker

Vis oppsummering:
- Antall funn per kategori
- Topp 3-5 viktigste funn
- Henvisning til full rapport

Spør:
> Vil du at jeg skal fikse noen av disse? Eller vil du se detaljer om spesifikke funn?

## Logg

Logg viktige funn til `workspace/memory/discoveries.jsonl`:
```json
{"timestamp": "...", "type": "review-finding", "content": "SQL injection i login.js:45", "context": "review", "severity": "critical"}
```

# Kodeagent - Claude Code Instructions

Du er en kodeagent som jobber med **eksisterende kodebaser**. Din jobb er å koble til en ekstern kodebase og hjelpe brukeren med å analysere, utvide og forbedre den.

## Viktig konsept

**Du kjører fra `kodeagent/`-mappen, men jobber med kode i en ekstern mappe.**

All konfigurasjon og dokumentasjon lagres her i `kodeagent/workspace/`, men kodeendringer gjøres i målmappen som brukeren har koblet til.

## Hvordan du starter

1. Bruk `/kodeagent:connect` for å koble til en kodebase
2. Bruk `/kodeagent:analyze` for å forstå kodebasen
3. Bruk de andre kommandoene for å jobbe med koden

## Tilgjengelige kommandoer

| Kommando | Beskrivelse |
|----------|-------------|
| `/kodeagent:connect` | Koble til en eksisterende kodebase |
| `/kodeagent:analyze` | Analyser kodebasen grundig |
| `/kodeagent:feature` | Planlegg og implementer ny funksjonalitet |
| `/kodeagent:review` | Gjør kodegjennomgang |
| `/kodeagent:fix` | Fiks bugs og issues |
| `/kodeagent:polars` | Polars-ekspert for effektiv databehandling |

## Mappestruktur

```
kodeagent/
├── CLAUDE.md                    # Denne filen
├── README.md                    # Brukerdokumentasjon
├── .claude/commands/            # Slash-kommandoer
├── templates/                   # Maler for dokumenter
└── workspace/                   # Arbeidsfiler
    ├── config.json              # Konfigurasjon (målmappe)
    ├── memory/                  # Læringslogg
    │   ├── decisions.jsonl      # Beslutninger tatt
    │   ├── errors.jsonl         # Feil og løsninger
    │   └── discoveries.jsonl    # Oppdagelser om kodebasen
    └── docs/                    # Generert dokumentasjon
        ├── analysis.md          # Kodebase-analyse
        └── features/            # Feature-dokumenter
```

## Memory-system

Logger viktige oppdagelser til `workspace/memory/`:

- **decisions.jsonl**: Arkitekturvalg og beslutninger
- **errors.jsonl**: Feil som oppstod og hvordan de ble løst
- **discoveries.jsonl**: Interessante funn om kodebasen

Hver linje er JSON med format:
```json
{"timestamp": "ISO-8601", "type": "...", "content": "...", "context": "..."}
```

## Arbeidsflyt

### Ved feature-utvikling
1. Les `workspace/docs/analysis.md` for kontekst
2. Sjekk `workspace/memory/` for tidligere læring
3. Lag feature-spec i `workspace/docs/features/`
4. Implementer med Human-in-the-loop (spør før store endringer)
5. Logg beslutninger og læring til memory

### Ved kodegjennomgang
1. Analyser målkodebasen systematisk
2. Generer rapport basert på `templates/code-review.md`
3. Prioriter funn etter alvorlighet

### Ved bugfiks
1. Forstå problemet
2. Finn rotårsak i målkodebasen
3. Implementer fiks
4. Logg til `workspace/memory/errors.jsonl`

## Viktige prinsipper

1. **Les før du skriver** - Forstå eksisterende kode før du endrer den
2. **Respekter mønstre** - Følg kodens eksisterende stil og arkitektur
3. **Human-in-the-loop** - Spør brukeren før store endringer
4. **Dokumenter læring** - Logg til memory for fremtidige økter
5. **Minimal endring** - Gjør kun det som er nødvendig

## Konfigurasjonsfil

`workspace/config.json` inneholder:
```json
{
  "target_path": "/absolutt/sti/til/kodebase",
  "connected_at": "ISO-8601 timestamp",
  "project_name": "prosjektnavn"
}
```

Les alltid denne filen først for å vite hvor målkodebasen er.

# Kodeagent - Claude Code Instructions

Du er en kodeagent som jobber med **eksisterende kodebaser**. Din jobb er å koble til en ekstern kodebase og hjelpe brukeren med å analysere, utvide og forbedre den.

## Viktig konsept

**Du kjører fra `kodeagent/`-mappen, men jobber med kode i en ekstern mappe.**

All konfigurasjon og dokumentasjon lagres her i `kodeagent/workspace/`, men kodeendringer gjøres i målmappen som brukeren har koblet til.

## Ved oppstart (VIKTIG - les dette etter /clear)

**Før du gjør noe annet, sjekk om det er et aktivt prosjekt:**

1. Les `workspace/active.json` - inneholder navnet på aktivt prosjekt
2. Les `workspace/projects/<prosjekt>/config.json` for prosjektinfo
3. Les `workspace/projects/<prosjekt>/docs/analysis.md` hvis den finnes
4. Sjekk `workspace/projects/<prosjekt>/memory/` for tidligere læring

Hvis active.json finnes, informer brukeren:
> Aktivt prosjekt: [project_name]
> Sti: [target_path]
> Koblet til: [connected_at]

## Første gangs oppsett

Hvis `workspace/active.json` ikke finnes:
1. Bruk `/connect` for å koble til en kodebase
2. Bruk `/analyze` for å forstå kodebasen
3. Bruk de andre kommandoene for å jobbe med koden

## Tilgjengelige kommandoer

| Kommando | Beskrivelse |
|----------|-------------|
| `/connect` | Koble til en eksisterende kodebase |
| `/analyze` | Analyser kodebasen grundig |
| `/feature` | Planlegg og implementer ny funksjonalitet |
| `/review` | Gjør kodegjennomgang |
| `/fix` | Fiks bugs og issues |
| `/commit` | Commit i tilkoblet prosjekt (--push, --amend) |
| `/polars` | Polars-ekspert for effektiv databehandling |

## Mappestruktur

```
kodeagent/
├── CLAUDE.md                    # Denne filen
├── README.md                    # Brukerdokumentasjon
├── .claude/commands/            # Slash-kommandoer
├── templates/                   # Maler for dokumenter
└── workspace/
    ├── active.json              # Peker til aktivt prosjekt
    ├── history.json             # Historikk (siste 10 prosjekter)
    ├── docs/                    # Global dokumentasjon
    │   └── polars-reference.md  # Cachet polars-docs
    └── projects/                # Isolert per prosjekt
        └── <prosjekt-navn>/
            ├── config.json      # Prosjekt-konfigurasjon
            ├── memory/          # Læringslogg
            │   ├── decisions.jsonl
            │   ├── errors.jsonl
            │   └── discoveries.jsonl
            └── docs/            # Prosjekt-dokumentasjon
                ├── analysis.md
                └── features/
```

## Memory-system

Logger viktige oppdagelser til `workspace/projects/<prosjekt>/memory/`:

- **decisions.jsonl**: Arkitekturvalg og beslutninger
- **errors.jsonl**: Feil som oppstod og hvordan de ble løst
- **discoveries.jsonl**: Interessante funn om kodebasen

Hver linje er JSON med format:
```json
{"timestamp": "ISO-8601", "type": "...", "content": "...", "context": "..."}
```

## Arbeidsflyt

### Ved feature-utvikling
1. Les `workspace/projects/<prosjekt>/docs/analysis.md` for kontekst
2. Sjekk `workspace/projects/<prosjekt>/memory/` for tidligere læring
3. Lag feature-spec i `workspace/projects/<prosjekt>/docs/features/`
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
4. Logg til prosjektets `memory/errors.jsonl`

## Viktige prinsipper

1. **Les før du skriver** - Forstå eksisterende kode før du endrer den
2. **Respekter mønstre** - Følg kodens eksisterende stil og arkitektur
3. **Human-in-the-loop** - Spør brukeren før store endringer
4. **Dokumenter læring** - Logg til memory for fremtidige økter
5. **Minimal endring** - Gjør kun det som er nødvendig

## Konfigurasjonsfiler

### workspace/active.json
```json
{
  "project": "prosjekt-navn"
}
```

### workspace/history.json
```json
{
  "projects": [
    {
      "name": "prosjekt-navn",
      "path": "/absolutt/sti",
      "last_used": "ISO-8601",
      "connected_at": "ISO-8601"
    }
  ]
}
```

### workspace/projects/<prosjekt>/config.json
```json
{
  "target_path": "/absolutt/sti/til/kodebase",
  "connected_at": "ISO-8601 timestamp",
  "project_name": "prosjektnavn"
}
```

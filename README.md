# Kodeagent

En Claude Code-arbeidsflyt for å jobbe med **eksisterende kodebaser**.

## Hva er dette?

Kodeagent er et sett med slash-kommandoer som lar Claude Code koble til og jobbe med en hvilken som helst eksisterende kodebase. I motsetning til "idea-to-product" som bygger fra scratch, fokuserer Kodeagent på å:

- Analysere eksisterende kode
- Legge til nye features
- Gjøre kodegjennomgang
- Fikse bugs

## Kom i gang

### 1. Åpne Claude Code i denne mappen

```bash
cd kodeagent
claude
```

### 2. Koble til din kodebase

```
/kodeagent:connect
```

Du blir spurt om absolutt sti til kodebasen du vil jobbe med.

### 3. Analyser kodebasen

```
/kodeagent:analyze
```

Dette gir Claude en grundig forståelse av koden din.

### 4. Begynn å jobbe

Nå kan du bruke de andre kommandoene:

- `/kodeagent:feature` - Legg til ny funksjonalitet
- `/kodeagent:review` - Gjør kodegjennomgang
- `/kodeagent:fix` - Fiks bugs

## Kommandoer

| Kommando | Beskrivelse |
|----------|-------------|
| `/kodeagent:connect` | Koble til en kodebase ved å oppgi sti |
| `/kodeagent:analyze` | Analyser struktur, språk, rammeverk og mønstre |
| `/kodeagent:feature` | Planlegg og implementer ny feature |
| `/kodeagent:review` | Gjennomgå koden for bugs, sikkerhet, forbedringer |
| `/kodeagent:fix` | Beskriv en bug og få den fikset |

## Mappestruktur

```
kodeagent/
├── CLAUDE.md                    # Instruksjoner til Claude
├── README.md                    # Denne filen
├── .claude/commands/            # Slash-kommandoer
├── templates/                   # Maler for dokumenter
└── workspace/                   # Genereres av /connect
    ├── config.json              # Konfigurasjon
    ├── memory/                  # Læringslogg
    └── docs/                    # Generert dokumentasjon
```

## Hvordan det fungerer

1. **Kobling**: `/connect` lagrer stien til målmappen i `workspace/config.json`
2. **Analyse**: Claude leser og forstår kodebasen, lagrer i `workspace/docs/`
3. **Memory**: Beslutninger og læring lagres i `workspace/memory/`
4. **Arbeid**: Claude jobber med koden i målmappen, dokumenterer her

## Tips

- Kjør `/analyze` etter `/connect` for best resultat
- Claude husker tidligere arbeid via memory-systemet
- Feature-specs lagres for fremtidig referanse
- Bruk `/review` før viktige releases

## Eksempel-workflow

```
# Koble til prosjektet ditt
/kodeagent:connect
> /home/bruker/mitt-prosjekt

# La Claude forstå koden
/kodeagent:analyze

# Legg til en feature
/kodeagent:feature
> Jeg vil legge til brukerautentisering

# Før release, gjør review
/kodeagent:review
```

## Tilpasning

Du kan tilpasse malene i `templates/` for å matche dine behov:

- `feature-spec.md` - Hvordan features dokumenteres
- `code-review.md` - Format for reviews
- `analysis-report.md` - Struktur for analyser

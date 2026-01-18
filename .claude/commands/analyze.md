# Analyser kodebasen

Du skal gjøre en grundig analyse av den tilkoblede kodebasen.

## Forutsetninger

Les først `workspace/config.json` for å finne målmappen. Hvis filen ikke finnes, be brukeren kjøre `/kodeagent:connect` først.

## Analysesteg

### 1. Grunnleggende info

Kjør i målmappen:
- `ls -la` - Se rotstruktur
- `find . -name "*.json" -path "*/." -o -name "package.json" -o -name "Cargo.toml" -o -name "go.mod" -o -name "requirements.txt" | head -20` - Finn prosjektfiler

### 2. Identifiser språk og rammeverk

Se etter:
- **JavaScript/TypeScript**: package.json, tsconfig.json
- **Python**: requirements.txt, pyproject.toml, setup.py
- **Rust**: Cargo.toml
- **Go**: go.mod
- **Java**: pom.xml, build.gradle

### 3. Mappestruktur

Kartlegg hovedmapper:
- src/, lib/, app/
- tests/, test/
- docs/
- config/

### 4. Arkitekturmønstre

Identifiser:
- MVC, MVVM, Clean Architecture?
- Monolitt eller microservices?
- REST, GraphQL, gRPC?
- Database-typer

### 5. Kodestil

Se etter:
- Linting-konfig (.eslintrc, .prettierrc, ruff.toml)
- Formattering-regler
- Navnekonvensjoner

### 6. Viktige filer

Finn og les:
- README.md
- CONTRIBUTING.md
- Hovedentry-point

## Generer rapport

Lag `workspace/docs/analysis.md` basert på malen `templates/analysis-report.md`.

Rapporten skal inneholde:
1. Prosjektoversikt
2. Teknologi-stack
3. Mappestruktur
4. Arkitektur
5. Viktige filer
6. Kjørekommandoer (build, test, start)
7. Anbefalinger

## Logg oppdagelser

Logg interessante funn til `workspace/memory/discoveries.jsonl`:

```json
{"timestamp": "...", "type": "discovery", "content": "Prosjektet bruker Clean Architecture", "context": "analyze"}
```

## Output

Vis en oppsummering til brukeren og bekreft at full analyse er lagret i `workspace/docs/analysis.md`.

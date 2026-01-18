# Koble til eksisterende kodebase

Du skal koble til en eksisterende kodebase som brukeren vil jobbe med.

## Steg

### 1. Spør om sti

Spør brukeren om den absolutte stien til kodebasen de vil jobbe med.

Eksempel på spørsmål:
> Hvilken kodebase vil du jobbe med? Oppgi absolutt sti (f.eks. `/home/bruker/mitt-prosjekt`)

### 2. Verifiser at mappen eksisterer

Bruk `ls` for å verifisere at mappen finnes og inneholder kode:
```bash
ls -la /oppgitt/sti
```

Hvis mappen ikke finnes, informer brukeren og be om riktig sti.

### 3. Opprett workspace-struktur

Hvis `workspace/` ikke allerede er satt opp, opprett:

```bash
mkdir -p workspace/memory workspace/docs/features
```

### 4. Opprett config.json

Lag `workspace/config.json` med følgende struktur:

```json
{
  "target_path": "/absolutt/sti/fra/bruker",
  "connected_at": "YYYY-MM-DDTHH:mm:ss.sssZ",
  "project_name": "mappenavn"
}
```

Bruk faktisk timestamp og trekk ut mappenavn fra stien.

### 5. Initialiser memory-filer

Opprett tomme JSONL-filer:
- `workspace/memory/decisions.jsonl`
- `workspace/memory/errors.jsonl`
- `workspace/memory/discoveries.jsonl`

### 6. Bekreft til brukeren

Gi en oppsummering:

> ✓ Koblet til: [project_name]
> Sti: [target_path]
>
> Neste steg:
> - Kjør `/kodeagent:analyze` for å analysere kodebasen
> - Eller start direkte med `/kodeagent:feature`, `/kodeagent:review`, eller `/kodeagent:fix`

## Feilhåndtering

- Hvis stien er relativ, be om absolutt sti
- Hvis mappen ikke finnes, be om korrekt sti
- Hvis mappen er tom, advar brukeren men fortsett

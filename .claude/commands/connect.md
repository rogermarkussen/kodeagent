# Koble til eksisterende kodebase

Du skal koble til en eksisterende kodebase som brukeren vil jobbe med.

## Steg

### 1. Sjekk historikk

Les `workspace/history.json` for å se om det finnes tidligere prosjekter.

Hvis historikk finnes og bruker ikke oppga en sti som argument:
- Vis liste over siste 10 prosjekter (nyeste først)
- Marker forrige prosjekt som default
- La brukeren velge fra listen ELLER oppgi ny sti

Eksempel på visning:
```
Tidligere prosjekter:

1. [DEFAULT] nytt-testprod (/home/bruker/dev/nytt-testprod) - sist brukt: 18.01.2026
2. mitt-api (/home/bruker/dev/mitt-api) - sist brukt: 15.01.2026
3. frontend-app (/home/bruker/dev/frontend-app) - sist brukt: 10.01.2026

Velg nummer (1-3) eller oppgi ny sti:
```

### 2. Håndter valg

**Hvis bruker velger fra listen:**
- Hent prosjektinfo fra historikken
- Oppdater `last_used` i history.json
- Sett prosjektet som aktivt

**Hvis bruker oppgir ny sti:**
- Verifiser at mappen eksisterer med `ls -la`
- Hvis mappen ikke finnes, informer og be om riktig sti

### 3. Opprett/oppdater workspace-struktur

For nytt prosjekt, opprett:
```bash
mkdir -p workspace/projects/<prosjekt-navn>/memory
mkdir -p workspace/projects/<prosjekt-navn>/docs/features
```

### 4. Opprett/oppdater config.json

Lag `workspace/projects/<prosjekt-navn>/config.json`:
```json
{
  "target_path": "/absolutt/sti/fra/bruker",
  "connected_at": "YYYY-MM-DDTHH:mm:ss.sssZ",
  "project_name": "mappenavn"
}
```

### 5. Oppdater active.json

Sett aktivt prosjekt i `workspace/active.json`:
```json
{
  "project": "prosjekt-navn"
}
```

### 6. Oppdater history.json

Legg til/oppdater prosjektet i `workspace/history.json`:
- Sett `last_used` til nåværende timestamp
- Flytt prosjektet til toppen av listen
- Behold maks 10 prosjekter (fjern eldste hvis over)

```json
{
  "projects": [
    {
      "name": "prosjekt-navn",
      "path": "/absolutt/sti",
      "last_used": "YYYY-MM-DDTHH:mm:ss.sssZ",
      "connected_at": "YYYY-MM-DDTHH:mm:ss.sssZ"
    }
  ]
}
```

### 7. Initialiser memory-filer (kun for nye prosjekter)

Opprett tomme JSONL-filer:
- `workspace/projects/<prosjekt>/memory/decisions.jsonl`
- `workspace/projects/<prosjekt>/memory/errors.jsonl`
- `workspace/projects/<prosjekt>/memory/discoveries.jsonl`

### 8. Bekreft til brukeren

Gi en oppsummering:

> ✓ Koblet til: [project_name]
> Sti: [target_path]
>
> Neste steg:
> - Kjør `/analyze` for å analysere kodebasen
> - Eller start direkte med `/feature`, `/review`, eller `/fix`

## Feilhåndtering

- Hvis stien er relativ, be om absolutt sti
- Hvis mappen ikke finnes, be om korrekt sti
- Hvis mappen er tom, advar brukeren men fortsett
- Hvis prosjekt allerede finnes i historikk men med annen sti, spør om oppdatering

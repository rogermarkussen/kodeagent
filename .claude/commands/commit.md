# Commit i tilkoblet prosjekt

Denne kommandoen håndterer git-operasjoner i det tilkoblede prosjektet - ALDRI i kodeagent-mappen.

## KRITISK SIKKERHETSREGEL

**ALLE git-kommandoer SKAL kjøres med `-C <target_path>`:**
```bash
git -C /sti/til/prosjekt status
git -C /sti/til/prosjekt add .
git -C /sti/til/prosjekt commit -m "melding"
```

**ALDRI kjør git-kommandoer uten `-C` flagget!**

## Argumenter

| Argument | Beskrivelse |
|----------|-------------|
| (ingen) | Commit endringer |
| `--push` | Commit og push |
| `--amend` | Amend siste commit |

## Steg

### 1. Finn tilkoblet prosjekt

Les `workspace/active.json` for å finne aktivt prosjekt:
```json
{
  "project": "prosjekt-navn"
}
```

Les deretter `workspace/projects/<prosjekt>/config.json` for target_path.

**Hvis ingen prosjekt er tilkoblet:**
> Ingen prosjekt er tilkoblet. Kjør `/connect` først.

### 2. Verifiser at vi IKKE committer i kodeagent

Sjekk at target_path IKKE er kodeagent-mappen:
```bash
# Hvis target_path inneholder "kodeagent" - STOPP og advar
```

### 3. Sjekk git status

```bash
git -C <target_path> status
git -C <target_path> diff --stat
git -C <target_path> log --oneline -5
```

### 4. Analyser endringer

- Hvis ingen endringer: informer brukeren og avslutt
- Hvis endringer: vis oppsummering og foreslå commit-melding

### 5. Lag commit

```bash
git -C <target_path> add .
git -C <target_path> commit -m "$(cat <<'EOF'
Commit-melding her

Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>
EOF
)"
```

### 6. Push (hvis --push)

```bash
git -C <target_path> push
```

### 7. Amend (hvis --amend)

```bash
git -C <target_path> commit --amend -m "$(cat <<'EOF'
Ny commit-melding

Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>
EOF
)"
```

### 8. Logg til memory

Logg commit til `workspace/projects/<prosjekt>/memory/decisions.jsonl`:
```json
{"timestamp": "...", "type": "commit", "content": "Commit: <melding>", "context": "commit"}
```

### 9. Bekreft

> ✓ Commit opprettet i [project_name]
> Melding: [commit message]
> Hash: [short hash]
> [Pushet til origin] (hvis --push)

## Feilhåndtering

- Hvis ikke i git-repo: informer brukeren
- Hvis merge-konflikt: vis konflikter og be om hjelp
- Hvis push feiler: vis feilmelding og foreslå løsning
- **Hvis target_path er kodeagent-mappen: STOPP UMIDDELBART**

## Eksempel på bruk

```
/commit
→ Committer endringer i tilkoblet prosjekt

/commit --push
→ Committer og pusher

/commit --amend
→ Endrer siste commit
```

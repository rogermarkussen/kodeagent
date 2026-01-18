# Kodebase-analyse: ekomadmin

**Analysert:** 2026-01-18
**Sti:** /home/rmarkussen/dev/ekomadmin

---

## 1. Prosjektoversikt

Internt admin-verktøy for administrasjon av telekomleverandører (tilbydere), kontakter, kommentarer og rapportstatus. Applikasjonen er koblet til en Azure PostgreSQL-database.

**Status:** Ferdig utviklet (alle 7 faser komplett iht. utviklingsplanen i CLAUDE.md)

---

## 2. Teknologi-stack

| Kategori | Teknologi | Versjon |
|----------|-----------|---------|
| **Framework** | Next.js (App Router) | 16.1.1 |
| **Språk** | TypeScript | ^5 |
| **Runtime** | Bun | - |
| **Database** | Azure PostgreSQL | - |
| **ORM** | Prisma | 7.2.0 |
| **Autentisering** | Auth.js (NextAuth v5 beta) | 5.0.0-beta.30 |
| **UI-bibliotek** | shadcn/ui | - |
| **Styling** | Tailwind CSS | v4 |
| **Ikoner** | lucide-react | 0.562.0 |
| **Skjema** | React Hook Form + Zod | 7.70.0 / 4.3.5 |
| **Tabeller** | TanStack React Table | 8.21.3 |
| **Toast** | Sonner | 2.0.7 |
| **Excel-eksport** | xlsx | 0.18.5 |
| **Deploy** | Azure Web App for Containers | - |

---

## 3. Mappestruktur

```
src/
├── app/
│   ├── (dashboard)/           # Autentiserte ruter med sidebar-layout
│   │   ├── layout.tsx         # Dashboard-layout
│   │   ├── page.tsx           # Hovedside
│   │   ├── tilbydere/         # Tilbydere-modul (CRUD + fusjoner)
│   │   ├── kontakter/         # Kontakter-modul (CRUD + Excel-eksport)
│   │   ├── kommentarer/       # Kommentarer-modul (CRUD)
│   │   ├── gruppering/        # Gruppering-modul
│   │   ├── rapportstatus/     # Rapportstatus-modul
│   │   └── ekom-config/       # Placeholder for fremtidig modul
│   ├── api/auth/[...nextauth]/ # Auth.js route handler
│   ├── login/                  # Login-side (utenfor dashboard)
│   └── actions/                # Server Actions
│       ├── tilbydere.ts       # 8.8 KB
│       ├── kontakter.ts       # 5.4 KB
│       ├── kommentarer.ts     # 4.5 KB
│       ├── gruppering.ts      # 12.6 KB
│       ├── rapportstatus.ts   # 5.2 KB
│       └── *.types.ts         # Type-definisjoner
├── components/
│   ├── ui/                    # 25 shadcn/ui-komponenter
│   ├── sidebar.tsx            # Sammenleggbar navigasjon
│   ├── header.tsx             # Header med brukerinfo
│   ├── app-layout.tsx         # Layout-wrapper
│   ├── data-table.tsx         # Gjenbrukbar DataTable
│   ├── *-form.tsx             # Skjema-komponenter
│   └── *-skeleton.tsx         # Loading-states
├── lib/
│   ├── auth.ts                # Auth.js-konfig med email allowlist
│   ├── prisma.ts              # Prisma-klient singleton (Azure SSL)
│   └── utils.ts               # Utilities
├── generated/prisma/          # Generert Prisma-klient (gitignored)
└── proxy.ts                   # Auth proxy (Next.js 16)

prisma/
└── schema.prisma              # Database-skjema (16 modeller)
```

---

## 4. Arkitektur

### 4.1 Mønster
- **App Router** med route groups `(dashboard)` for autentiserte sider
- **Server Actions** for alle database-mutasjoner
- **Server Components** som default, Client Components ved behov
- **Gjenbrukbare komponenter** (DataTable, skjemaer, dialogs)

### 4.2 Autentisering
- **Auth.js** med Google OAuth provider
- **Email allowlist** via `ALLOWED_EMAILS` miljøvariabel
- **Dev login** for Playwright-testing (kun i development)
- **JWT-strategi** for sessions

### 4.3 Database-modeller

**Forretningslogikk:**
- `tilbydere` - Telekomleverandører (hovedentitet)
- `kontakter` - Kontaktpersoner (FK: tilb_id)
- `kommentarer` - Kommentarer (FK: tilb_id, nullable)
- `fusjoner` - Fusjoner mellom tilbydere
- `gruppering` - Tilbydergrupper
- `rapportstatus` - Rapporteringsstatus per år

**Ekom-data:**
- `ekomspm`, `ekomsvar`, `ekomsession`, `ekomleveringer`
- `aktiv_ekom`, `ekom_config`
- `rapportdata`, `rapportsession`

**Auth.js:**
- `User`, `Account`, `Session`, `VerificationToken`

### 4.4 UI-layout
- **Sidebar:** Sammenleggbar (256px → 64px), tooltips når collapsed
- **Header:** Logo + brukeravatar med dropdown
- **Mobil:** Separat mobilmeny (Sheet-komponent)
- **Loading:** Skeleton-komponenter per side

---

## 5. Viktige filer

| Fil | Beskrivelse |
|-----|-------------|
| `CLAUDE.md` | Komplett prosjektdokumentasjon |
| `APP_SPECS.md` | Modulkrav og features |
| `src/lib/auth.ts` | Auth-konfigurasjon |
| `src/lib/prisma.ts` | Database-tilkobling |
| `src/components/data-table.tsx` | Gjenbrukbar tabell |
| `prisma/schema.prisma` | Database-skjema |

---

## 6. Kjørekommandoer

```bash
# Utvikling
bun dev              # Start dev-server (port 3000)
bun build            # Produksjonsbygg
bun start            # Start produksjonsserver
bun lint             # Kjør ESLint

# Database
bunx prisma generate # Generer Prisma-klient
bunx prisma db push  # Push skjema til database
bunx prisma db pull  # Pull skjema fra database
bunx prisma studio   # Åpne Prisma Studio

# Docker/Deploy
podman build -t ekomadmin .
podman push ekomadminacr.azurecr.io/ekomadmin:latest
az webapp restart --name ekomadmin --resource-group romressurser
```

---

## 7. Nøkkelkonfigurasjon

### Miljøvariabler
- `DATABASE_URL` - PostgreSQL connection string
- `AUTH_SECRET` - Auth.js secret
- `AUTH_GOOGLE_ID` / `AUTH_GOOGLE_SECRET` - Google OAuth
- `ALLOWED_EMAILS` - Kommaseparert liste over tillatte e-poster
- `ENABLE_DEV_LOGIN` - Aktiver dev-login for testing

### Viktige merknader
- **Ikke start dev-server som bakgrunnsprosess** - brukeren har alltid serveren kjørende
- **Passord-feltet i tilbydere er deprecated** - ikke vis/rediger
- **Prisma 7** bruker `@prisma/adapter-pg` med SSL for Azure

---

## 8. Modulstatus

| Modul | Status | Beskrivelse |
|-------|--------|-------------|
| Tilbydere | ✅ Ferdig | Full CRUD, fusjoner, relaterte data |
| Kontakter | ✅ Ferdig | CRUD, Excel-eksport |
| Kommentarer | ✅ Ferdig | CRUD, datofilter |
| Gruppering | ✅ Ferdig | Visualisering og redigering |
| Rapportstatus | ✅ Ferdig | Statusoppdatering, notat inline |
| Ekom Config | 🔜 Placeholder | Reservert for fremtidig utvikling |

---

## 9. Anbefalinger for videre utvikling

1. **Ekom Config-modulen** er neste naturlige steg
2. **Tester** - ingen automatiske tester i prosjektet ennå
3. **Error boundaries** er implementert, men kan utvides
4. **Caching** - vurder React Query eller SWR for datahenting

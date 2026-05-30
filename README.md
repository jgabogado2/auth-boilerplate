# Auth Boilerplate

A production-ready **authentication boilerplate** for Next.js. Drop it in, point it at your own Supabase project + Google OAuth credentials, and start building your app on top of a fully wired auth layer.

> The goal is to never write the same auth code twice. Clone this, rename it, swap the env vars, and ship.

---

## What's included

- **Next.js 16** (App Router) + **React 19** + **TypeScript**
- **NextAuth.js** with **Google OAuth** provider
- **Supabase** as the database (with RLS-friendly server + browser clients)
- **Role-based access control** — `admin`, `manager`, `user` (hierarchical, see `lib/auth.types.ts`)
- **Whitelist system** — only pre-approved emails can sign in (see `docs/WHITELIST_SYSTEM.md`)
- **Super-admin bootstrap flow** (see `docs/SUPER_ADMIN_SETUP.md`)
- **Middleware-protected routes** with role checks (see `middleware.ts`)
- **Dashboard scaffold** under `app/(dashboard)` with admin, projects, and settings sections
- **shadcn/ui** + **Tailwind CSS v4** for the UI layer
- SQL migrations under `supabase/migrations/`

---

## Tech stack

| Layer        | Choice                                   |
| ------------ | ---------------------------------------- |
| Framework    | Next.js 16 (App Router)                  |
| Auth         | NextAuth.js + Google provider            |
| Database     | Supabase (Postgres + RLS)                |
| UI           | Tailwind CSS v4, shadcn/ui, Radix UI     |
| Icons        | lucide-react                             |
| Lang         | TypeScript                               |

---

## Prerequisites

Before you start, make sure you have:

1. **Node.js 20+** and **pnpm** (or npm / yarn / bun)
2. A **Supabase project** — [supabase.com](https://supabase.com)
3. **Google OAuth credentials** — [Google Cloud Console](https://console.cloud.google.com/apis/credentials)

Detailed prerequisites are in [`docs/SETUP_PREREQUISITES.md`](docs/SETUP_PREREQUISITES.md).

---

## Getting started

### 1. Use this as a template

```bash
# Clone or use as a GitHub template
git clone https://github.com/jgabogado2/auth-boilerplate.git my-new-app
cd my-new-app
rm -rf .git && git init
```

### 2. Install dependencies

```bash
pnpm install
# or: npm install / yarn / bun install
```

### 3. Configure environment variables

Copy the template and fill in your own values:

```bash
cp .env.example .env.local
```

Then edit `.env.local`:

```env
# Auth.js
AUTH_SECRET=          # generate with: openssl rand -base64 32
AUTH_GOOGLE_ID=
AUTH_GOOGLE_SECRET=

# Supabase
NEXT_PUBLIC_SUPABASE_URL=https://<project-id>.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=    # server-only, bypasses RLS
```

### 4. Run database migrations

Apply the SQL in `supabase/migrations/migrations.sql` to your Supabase project. See [`docs/DATABASE_MIGRATIONS.md`](docs/DATABASE_MIGRATIONS.md) for options (Supabase Dashboard SQL editor, CLI, etc.).

### 5. Bootstrap your first super-admin

The whitelist system blocks all sign-ins by default. Add yourself first:

```sql
insert into whitelist (email, role) values ('you@example.com', 'admin');
```

Full walkthrough: [`docs/SUPER_ADMIN_SETUP.md`](docs/SUPER_ADMIN_SETUP.md).

### 6. Run the dev server

```bash
pnpm dev
```

Open [http://localhost:3000](http://localhost:3000) and sign in with Google.

---

## Project structure

```
.
├── app/
│   ├── (dashboard)/        # Protected app shell (admin, projects, settings)
│   ├── api/auth/           # NextAuth route handlers
│   ├── signin/             # Sign-in page
│   └── unauthorized/       # 403 page for failed role checks
├── components/             # shadcn/ui + custom components
├── lib/
│   ├── auth.config.ts      # NextAuth config (providers, callbacks)
│   ├── auth-server.ts      # Server-side auth helpers
│   ├── auth-utils.ts       # Role/permission helpers
│   ├── auth.types.ts       # Role hierarchy + type guards
│   └── supabase.ts         # Browser + server Supabase clients
├── middleware.ts           # Route protection + role checks
├── supabase/migrations/    # SQL migrations
└── docs/                   # Architecture + setup guides
```

---

## Documentation

Everything non-obvious lives in [`docs/`](docs/):

| Doc                                                          | What it covers                              |
| ------------------------------------------------------------ | ------------------------------------------- |
| [`AUTHENTICATION_BLUEPRINT.md`](docs/AUTHENTICATION_BLUEPRINT.md) | End-to-end auth flow + design rationale |
| [`SETUP_PREREQUISITES.md`](docs/SETUP_PREREQUISITES.md)      | Detailed prereqs (Supabase, Google OAuth)   |
| [`SUPER_ADMIN_SETUP.md`](docs/SUPER_ADMIN_SETUP.md)          | Bootstrapping the first admin               |
| [`WHITELIST_SYSTEM.md`](docs/WHITELIST_SYSTEM.md)            | How the email whitelist works               |
| [`ROLE_PERMISSIONS_MINIMAL.md`](docs/ROLE_PERMISSIONS_MINIMAL.md) | Role + permission model                 |
| [`DATABASE_MIGRATIONS.md`](docs/DATABASE_MIGRATIONS.md)      | How to apply / write migrations             |
| [`MIGRATION_GUIDE.md`](docs/MIGRATION_GUIDE.md)              | Upgrading an existing project to this setup |
| [`COMMIT_GUIDELINES.md`](docs/COMMIT_GUIDELINES.md)          | Commit message conventions                  |

---

## Customizing for your project

When you start a new project from this boilerplate:

1. **Rename** in `package.json` (`name` field)
2. **Update** `app/(dashboard)/layout.tsx` branding
3. **Add your own tables** as new files in `supabase/migrations/`
4. **Add your own roles** (or keep `admin` / `manager` / `user`) in `lib/auth.types.ts`
5. **Add your own routes** under `app/(dashboard)/` — they're protected by default

---

## Scripts

```bash
pnpm dev      # Start dev server (http://localhost:3000)
pnpm build    # Production build
pnpm start    # Run the production build
pnpm lint     # ESLint
```

---

## License

Internal use. Adapt freely.

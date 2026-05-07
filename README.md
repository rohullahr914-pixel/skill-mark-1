# SkillMark MVP

SkillMark is a Next.js SaaS MVP for verifying practical Python skills through real browser-based coding challenges. Users sign up, solve Python challenges in Monaco Editor, submit code, and receive a public verification badge with a QR code when all hidden tests pass.

## Stack

- Next.js 14 App Router
- TypeScript
- Tailwind CSS with shadcn/ui-style primitives
- Prisma and PostgreSQL
- NextAuth credentials auth
- Monaco Editor
- Docker sandbox for Python execution
- QR code generation for public badge pages

## Prerequisites

- Node.js 20+
- Docker Desktop
- npm

## Local setup

1. Start PostgreSQL:

```bash
docker compose up -d db
```

2. Install dependencies:

```bash
npm install
```

3. Apply the Prisma migration:

```bash
npx prisma migrate dev
```

4. Start the app:

```bash
npm run dev
```

The dev command automatically seeds demo data when plans or challenges are missing.

Open `http://localhost:3000`.

## Demo account

- Email: `demo@skillmark.dev`
- Password: `password123`

## Seeded challenges

- Two Sum
- Normalize Palindrome
- Merge Intervals

Each challenge includes visible example tests and hidden tests. Hidden inputs and expected outputs are never sent to the browser.

## Python sandbox

Submissions are evaluated by `lib/challenge-runner.ts` using Docker:

- `--network none`
- `--memory 128m`
- `--cpus 0.5`
- `--pids-limit 64`
- `--read-only`
- isolated temp directory mounted read-only
- tmpfs `/tmp`
- hard overall timeout and per-test timeout

The app never runs submitted code directly on the host.

The runner uses the Docker image `python:3.12-slim`. Override it with:

```bash
SKILLMARK_PYTHON_IMAGE="python:3.12-slim"
```

## Useful commands

```bash
npm run db:seed
npm run db:studio
npm run lint
npm run build
```

## Production notes

This MVP uses credentials auth and mock billing. Before production, replace mock checkout with a real billing provider, rotate `NEXTAUTH_SECRET`, configure managed PostgreSQL, add rate limiting to submission endpoints, and consider running code execution in a dedicated worker service instead of inside the Next.js server process.

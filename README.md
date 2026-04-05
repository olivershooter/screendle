<p align="center">
  <img src="assets/screendle-icon.svg" alt="Screendle" width="120" />
</p>

<h1 align="center">Screendle</h1>

<p align="center">
  <strong>Daily movie guessing game.</strong><br/>
  Get a vague clue, guess the film, unlock the next hint. Five guesses. One movie per day.<br/><br/>
  <a href="https://screendle.com">screendle.com</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/react-18-61DAFB?logo=react&logoColor=white" alt="React" />
  <img src="https://img.shields.io/badge/typescript-5-3178C6?logo=typescript&logoColor=white" alt="TypeScript" />
  <img src="https://img.shields.io/badge/express-4-000000?logo=express&logoColor=white" alt="Express" />
  <img src="https://img.shields.io/badge/postgres-16-4169E1?logo=postgresql&logoColor=white" alt="PostgreSQL" />
  <img src="https://img.shields.io/badge/vite-5-646CFF?logo=vite&logoColor=white" alt="Vite" />
</p>

---

## How it works

Each day there's a new mystery movie. You start with a plot synopsis that's been sanitized to strip out character names and anything too obvious. Guess wrong and you get the next clue:

| Guess | Clue revealed |
|:-----:|---------------|
| **1** | Plot synopsis (sanitized) |
| **2** | Release date + content rating |
| **3** | Runtime + genres |
| **4** | Box office + critic review |
| **5** | Lead actors + director |
| **X** | Answer revealed with poster |

Movies come from a pool of ~2,000 TMDB films (1,000+ votes each). A cron job queues up the next 7 days every Sunday at midnight UTC. No repeats.

## Stack

| Layer | Tech |
|-------|------|
| Frontend | React, TypeScript, Vite |
| Backend | Express, TypeScript |
| Database | PostgreSQL + `pg_trgm` fuzzy search |
| Data | TMDB, OMDB, OpenAI (plot sanitization) |
| Infra | Docker, npm workspaces monorepo |

Game progress lives in `localStorage` — no user accounts.

## Running locally

```bash
npm install
docker compose up -d          # postgres
npm run db:migrate
npm run db:seed               # pulls ~2000 movies from TMDB/OMDB
npm run dev                   # frontend + backend
```

<details>
<summary><strong>Environment variables</strong></summary>

Create a `.env` in the project root:

```
DATABASE_URL=postgresql://screendle:screendle@localhost:5432/screendle
TMDB_API_KEY=your_tmdb_api_key
OMDB_API_KEY=your_omdb_api_key
OMDB_API_KEY_2=optional_second_omdb_key
OPENAI_API_KEY=your_openai_api_key
CORS_ORIGIN=https://your-domain.com
UMAMI_DOMAIN=https://your-umami-instance.com
VITE_UMAMI_DOMAIN=https://your-umami-instance.com
VITE_UMAMI_WEBSITE_ID=your-umami-website-id
SENTRY_DSN=your_sentry_dsn
VITE_SENTRY_DSN=your_frontend_sentry_dsn
```

</details>

## Other commands

| Command | What it does |
|---------|-------------|
| `npm run build` | Build all packages |
| `npm run test` | Run all tests |
| `npm run db:migrate` | Run migrations |
| `npm run db:seed` | Seed the movie pool |

## License

All rights reserved. This software is proprietary and may not be copied, modified, or distributed without explicit permission.

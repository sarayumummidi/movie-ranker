# Reelrank (working title)

Rank the movies and TV shows you've watched without star ratings. Reelrank shows you two titles at a
time and asks "which did you like more?", then uses your answers to build your own ranked list.
You can also import your Netflix viewing history and get a yearly "Wrapped"-style recap.

> Status: early development. Nothing is deployed yet.

## Features (planned)

1. **Search**: find movies and shows through the TMDB API and save them (title, year, poster, media type).
2. **Ranking**: each pick places a title in your list using binary insertion, so ranking a new title
   takes about log₂(n) comparisons. Every user has their own ranking.
3. **Netflix import**: upload your viewing-history CSV. Each row becomes a job on a queue. Background
   workers match titles against TMDB (rate-limited and cached), with retries and idempotency so no row
   is processed twice.
4. **Wrapped**: a batch job that builds a yearly recap from your rankings and watch history.

## Stack

| Layer      | Choice                                                  |
|------------|---------------------------------------------------------|
| Backend    | Go (net/http or chi), pgx, golang-migrate              |
| Database   | PostgreSQL                                              |
| Cache      | Redis (TMDB lookups)                                    |
| Queue      | AWS SQS (LocalStack locally), separate Go worker processes |
| Frontend   | React + Vite                                            |
| Local dev  | Docker Compose (Postgres, Redis, LocalStack)            |
| Deploy     | Kubernetes (kind locally, then AWS), GitHub Actions CI/CD |

## Roadmap

- [ ] **Week 1**: core app: TMDB search, Postgres, ranking, React UI. *Milestone: rank 10 movies in the browser.*
- [ ] **Week 2**: CSV import, SQS queue, workers, Redis cache, retries and idempotency.
- [ ] **Week 3**: Kubernetes deploy, CI/CD, worker autoscaling on queue depth, first real users.
- [ ] **Week 4**: load-test a large import, kill workers mid-job, check for lost or duplicated jobs, record results. Build Wrapped.
- [ ] **Later**: a recommender trained on rankings.

## Local setup

Setup instructions will go here once the services exist. You'll need a TMDB API key in a `.env` file:

```
TMDB_API_KEY=your_key_here
```

`.env` is gitignored. Never commit it.

## Results

Real numbers only: users, import throughput, and failure-test results will be recorded here as they're measured.

| Metric | Value | Date measured |
|--------|-------|---------------|
| —      | —     | —             |

## Attribution

This product uses the TMDB API but is not endorsed or certified by TMDB.

## Claim comment

Hi, I'd like to take this one as my Path Review contribution.

The issue asks for a way for clients to register a callback URL and get a `POST` with the review payload once a long multi-repo review (30–90 seconds) finishes. I haven't reproduced anything yet. My next step is to set up the repo from its README, run a multi-repo review, and record how the caller finds out it's done today (polling, blocking, or nothing). I'll post that as a repro report here, with my environment, steps, and output, before I start on `api/routes/webhooks.py` and `core/services/webhook_service.py`.

## Reproduction comment

I reproduced the gap in #35. Nothing in the app ever tells a client that a review has finished. The only way to find out is to keep polling.

**Environment:** 
- Linux
- Python 3.12.3
- Docker Postgres 16.15
- FastAPI 0.142.1
- Uvicorn 0.54.0
- SQLAlchemy 2.1.1
- commit `2f4e82f`
- `LLM_PROVIDER=mock`.

**Steps:**
1. `cp .env.example .env`, `docker compose up -d`, created a venv, `pip install -e ".[dev]"`, then `alembic upgrade head` and `scripts/seed_db.py`. I skipped the frontend and pre-commit parts of `make setup`.
2. Started `uvicorn api.main:app`, plus a small local listener on `127.0.0.1:9999` to catch any callback.
3. Logged in as `user1@example.com`. Login is form-encoded (`username=...&password=...`), not JSON.
4. There's no route to list profiles, so I read a `profile_id` from the database with `psql`.
5. `POST /reviews` with `{"profile_id": "...", "callback_url": "http://127.0.0.1:9999/hook"}`.
6. Polled `GET /reviews/{id}/status` once a second for 5 seconds, then waited 5 more.

**What happened:**
- `POST /webhooks` returns 404. The OpenAPI route list has no webhook or callback route.
- The API accepted `callback_url` without an error and ignored it, because `ReviewCreate` only defines `profile_id`.
- The server log shows the whole pipeline running in the same second the review was created, ending in `review_processing_completed`. No outbound HTTP call is logged.
- The listener received 0 requests.
- `grep -rliE "webhook|callback|notify|notification" api/ core/` matches no files. `process_review()` in `core/services/review_service.py` sets `status="complete"`, commits, logs, and returns, with no notification step.

# Repository Guidelines

## Project Structure & Module Organization
- `backend/`: FastAPI service (`src/main.py`, routers in `src/routers/`, DB code in `src/db/`, helpers in `src/utils/`), built via `Dockerfile`.
- `frontend/app/`: Vue 3 + TypeScript UI (Vite); entry `src/main.ts`, views/components under `src/`.
- `llm-evaluation-system/`: automated red-teaming stack; run its own docker compose when needed.
- `docs/`: user manuals; add contributor-facing docs here.
- Root configs: `docker-compose*.yml`, `nginx/`, `images/`, `output/` (generated artifacts—keep out of git when large or sensitive).

## Build, Test, and Development Commands
- Full stack: `docker compose build && docker compose up` (Postgres 15432, FastAPI 8000, frontend 5173).
- Backend only: `pip install -r backend/requirements.txt` then `PYTHONPATH=backend uvicorn src.main:app --app-dir backend/src --reload`.
- Frontend dev: `cd frontend/app && npm install && npm run dev -- --host`; production bundle with `npm run build`, check with `npm run preview`.
- DB init outside compose: `PYTHONPATH=backend python backend/src/db/define_tables.py` against a running Postgres.

## Coding Style & Naming Conventions
- Python: PEP8, 4-space indent, type hints; modules snake_case; keep handlers thin, push logic to helpers/managers.
- Vue/TS: 2-space indent; components PascalCase (`MyPanel.vue`), composables `useX.ts`; prefer `<script setup lang="ts">`.
- Config: keep environment values in env files; never hardcode API keys or URLs.

## Testing Guidelines
- Backend: pytest configured in `pyproject.toml`; place specs in `backend/tests/` as `test_*.py`. Mock external APIs and point to disposable Postgres (compose service or localhost:15432).
- Frontend: no runner by default—add Vitest for new logic and colocate `.spec.ts` files under `src/`.
- Run tests before PRs; add regression cases with every bug fix.

## Commit & Pull Request Guidelines
- Conventional Commit prefixes (`feat:`, `fix:`, `chore:`, `docs:`); imperative mood, one logical change per commit.
- PRs: brief what/why, commands executed (`pytest`, `npm run build`), screenshots/GIFs for UI, linked issue, and rollout/rollback notes if needed.
- Keep diffs focused and update docs when endpoints, workflows, or screens change.

## Security & Configuration Tips
- Default compose credentials are public (`postgres/password`); override via env files for any shared environment.
- Never commit API keys (e.g., OpenAI) or private datasets; load secrets through env vars or secret managers.
- Keep large/generated artifacts in `output/` and ensure they stay git-ignored when sensitive.

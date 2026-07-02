# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository purpose

This is the public repository for Udacity's Full Stack Web Developer Nanodegree (nd0044 v2). It is a collection of largely independent starter-code projects, each with incomplete `@TODO`/`TODO` sections that students fill in — not a single deployed application. Treat each project under `projects/` as its own isolated codebase with its own dependencies, virtualenv, and run instructions; there is no shared build system, root package.json (aside from `package-lock.json` used for security auditing), or monorepo tooling tying them together.

## Projects

- **`projects/01_fyyur/starter_code`** — Flask + SQLAlchemy + PostgreSQL venue/artist/show booking site (C1: SQL and Data Modeling for the Web). Models and DB interactions in `app.py` are intentionally incomplete (`# TODO: connect to a local postgresql database`). Uses `Flask-Migrate` for schema migrations, `Flask-WTF`/`forms.py` for forms, Jinja templates under `templates/`.
- **`projects/02_trivia_api/starter`** — Split `backend` (Flask + SQLAlchemy + PostgreSQL API in `backend/flaskr/__init__.py`, models in `backend/models.py`) and `frontend` (Create React App, proxying to `http://127.0.0.1:5000/`). Backend tests live in `backend/test_flaskr.py` (unittest, expects a `trivia_test` Postgres DB restored from `backend/trivia.psql`).
- **`projects/03_coffee_shop_full_stack/starter_code`** — Split `backend` (Flask API in `backend/src/api.py`, Auth0/JWT RBAC logic in `backend/src/auth/auth.py`, SQLite models in `backend/src/database/models.py`) and `frontend` (Ionic + Angular 20 app). Auth flow requires an Auth0 tenant with `get:drinks-detail`/`post:drinks`/`patch:drinks`/`delete:drinks` permissions and Barista/Manager roles; Postman collection at `backend/udacity-fsnd-udaspicelatte.postman_collection.json` is used to validate JWTs per role.
- **`projects/capstone`** — Final capstone starter scaffold (`starter/app.py`, `starter/models.py`, `starter/test_app.py`, `starter/setup.sh`); intentionally minimal/empty, meant to be built out per-student. `heroku_sample/starter` is a separate Heroku deployment reference scaffold.
- **`BasicFlaskAuth/`** and **`FlaskRecap/`** — small standalone Flask exercises/recap scripts, not part of the numbered project sequence. `BasicFlaskAuth/app.py` contains Auth0 JWT verification boilerplate with literal `@TODO_REPLACE_WITH_YOUR_DOMAIN` placeholders (not valid Python until filled in).
- **`LocalStore/index.html`** — standalone HTML/JS localStorage exercise, no build step.

## Working in a Python (Flask) project

Each Flask project has its own `requirements.txt`. Always create/activate a project-local virtualenv before installing:

```bash
cd projects/02_trivia_api/starter/backend   # or 01_fyyur/starter_code, 03_.../backend, capstone/starter
python3 -m venv venv && source venv/bin/activate
pip install -r requirements.txt
```

Running the dev server (Flask 1.x/2.x style, varies per project — check the project's own README):

```bash
export FLASK_APP=app.py   # or flaskr / api.py, per-project
flask run --reload
```

Running tests (unittest-based, e.g. trivia backend):

```bash
python -m unittest test_flaskr.py          # all tests
python -m unittest test_flaskr.py -k test_name   # single test (Python 3.7+ needs TriviaTestCase.test_name form on older unittest)
```

PostgreSQL-backed projects (`01_fyyur`, `02_trivia_api`) expect a running local Postgres instance and a database restored from the provided `.psql` dump before the app/tests will work.

## Working in a JS/TS (frontend) project

Each frontend has its own `package.json`; install and run from within that project directory:

```bash
cd projects/02_trivia_api/starter/frontend   # CRA app
npm install
npm start        # dev server
npm test         # jest/react-scripts tests
npm run build

cd projects/03_coffee_shop_full_stack/starter_code/frontend   # Ionic/Angular app
npm install
npm start         # ng serve
npm test          # ng test (karma/jasmine)
npm run lint       # ng lint
npm run e2e
```

## Dependency security posture

Recent history on this repo (see `bd1db06`, `ffc9385`, prior batches) has been dependency-vulnerability remediation: upgrading the coffee shop frontend from Angular v7/v11 to Angular v20.3.25, and adding `overrides` blocks in `package.json` (both `02_trivia_api/starter/frontend` and `03_coffee_shop_full_stack/starter_code/frontend`) to pin vulnerable transitive dependencies to patched versions. When touching frontend dependencies:
- Prefer adding/adjusting an `overrides` entry over other workarounds when a transitive dependency has a CVE and the direct dependency hasn't released a fix.
- After changing dependencies, `npm install` and `npm audit` should be re-run to confirm no regressions/new advisories.
- These are Udacity-provided student starter projects, not production apps — functionality (the app still runs and the exercise is completable) takes priority over dependency freshness, but do not reintroduce known-vulnerable pinned versions.

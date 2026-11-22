# cyber-attacks-api

A FastAPI service that fetches recent attacks from the Radware live threat map API, returns them as JSON and stores them in a database through Tortoise ORM.

## Features

- `GET /` health check.
- `GET /attacks/{limit}` fetches `limit` recent attacks from `livethreatmap.radware.com`, saves each one to the database and returns them.
- `GET /attacks` returns all stored attacks.
- Each record has source country, destination country, millisecond, type, weight and attack time.
- Aerich migration for the `datamodel` table (PostgreSQL syntax).

## Tech stack

Python 3, FastAPI, Uvicorn, Tortoise ORM, Aerich, PostgreSQL, python-dotenv, requests.

## Structure

```
main.py                    FastAPI app and routes
database.py                Tortoise ORM config (reads DB_URL)
models.py                  DataModel table
migrations/models/         Aerich initial migration
requirements.txt
```

## Setup

Requirements: Python 3.9+, a PostgreSQL database.

```bash
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
```

Create a `.env` file:

| Variable | Purpose |
|----------|---------|
| `DB_URL` | Database connection string, for example `postgres://user:password@localhost:5432/attacks` |

## Run

```bash
python main.py          # serves on http://0.0.0.0:8000 with reload
```

Tables are created on startup (`generate_schemas=True`).

## Usage

```bash
curl http://localhost:8000/attacks/10   # fetch and store 10 attacks
curl http://localhost:8000/attacks      # list stored attacks
```

## Limitations

- Depends on an unofficial, undocumented Radware endpoint that may change or restrict access.
- Fetching the same attacks twice stores duplicates (no de-duplication).
- `GET /attacks` has no pagination.
- No tests.

## License

MIT

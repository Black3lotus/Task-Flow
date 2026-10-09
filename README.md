# TaskFlow

A simple task manager. Add tasks, mark them done, delete them, and filter by status. Tasks are stored in Supabase (Postgres), so they survive a page refresh.

**Stack:** React + Vite (frontend), FastAPI (backend), Supabase (database).

> Add a screenshot here: `docs/screenshot.png`

## Features

- Add a task with a title, priority (High / Medium / Low) and optional due date
- See all tasks, newest first
- Mark a task done or open again
- Delete a task
- Filter by All / Open / Done

## API

| Method | Path | What it does |
| --- | --- | --- |
| GET | `/tasks?status=all\|open\|done` | List tasks |
| POST | `/tasks` | Create a task |
| PATCH | `/tasks/{id}` | Update title, done, priority or due date |
| DELETE | `/tasks/{id}` | Delete a task |
| GET | `/health` | Health check |

Interactive docs: http://localhost:8000/docs

## Run it locally

### 1. Database (Supabase)

1. Create a project at https://supabase.com.
2. Open **SQL Editor**, paste the contents of `backend/schema.sql`, and run it.
3. Open **Project Settings -> API** and copy the project URL and the `service_role` key.

### 2. Backend

```bash
cd backend
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env             # then fill in the values
uvicorn main:app --reload
```

The API runs on http://localhost:8000.

### 3. Frontend

```bash
cd frontend
npm install
cp .env.example .env
npm run dev
```

Open http://localhost:5173.

## Environment variables

| File | Variable | Meaning |
| --- | --- | --- |
| `backend/.env` | `SUPABASE_URL` | Your Supabase project URL |
| `backend/.env` | `SUPABASE_KEY` | Supabase `service_role` key (backend only, never expose it) |
| `backend/.env` | `FRONTEND_ORIGIN` | Allowed CORS origin(s), e.g. `http://localhost:5173` |
| `frontend/.env` | `VITE_API_URL` | URL of the backend, e.g. `http://localhost:8000` |

`.env` files are git-ignored. Only the `.env.example` files are committed.

## Deploy (optional)

- **Backend on Render:** root directory `backend`, build command `pip install -r requirements.txt`, start command `uvicorn main:app --host 0.0.0.0 --port $PORT`. Add `SUPABASE_URL`, `SUPABASE_KEY` and `FRONTEND_ORIGIN` (your frontend's public URL) as environment variables.
- **Frontend on Vercel or Netlify:** root directory `frontend`, build command `npm run build`, output `dist`. Set `VITE_API_URL` to the Render URL.

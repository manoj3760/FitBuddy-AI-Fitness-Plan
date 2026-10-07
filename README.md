# FitBuddy: AI workout plans with Gemini

FastAPI + Gemini + SQLite + Jinja2. Enter your details, get a 7-day plan and a nutrition tip, then refine the plan with feedback.

## Run it

1. Get a free API key at https://aistudio.google.com/apikey
2. In this folder:

```bash
python -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env              # Windows: copy .env.example .env
# open .env and paste your key after GOOGLE_API_KEY=
uvicorn app.main:app --reload
```

3. Open http://127.0.0.1:8000 (API docs at /docs).

## Pages and routes

| Route | What it does |
|---|---|
| `GET /` | Details form |
| `POST /generate-workout` | Plan (Gemini Pro) + tip (Gemini Flash), saved to SQLite |
| `POST /submit-feedback` | Revises the latest plan from feedback; the original is kept |
| `GET /view-all-users` | Admin list of users, original and updated plans |
| `POST /delete-user` | Removes a user and their plans |

## Layout

```
app/  main.py  routes.py  database.py  schemas.py  config.py
      gemini_client.py  gemini_generator.py  gemini_flash_generator.py  updated_plan.py
templates/  index.html  result.html  all_users.html
static/     style.css
```

## Notes

- Models are set in `.env` (`PLAN_MODEL`, `TIP_MODEL`). If Google retires a model name, change it there.
- Set `ADMIN_KEY` in `.env` to protect the admin page; then open `/view-all-users?key=YOUR_KEY`.
- Never commit `.env`. It is already in `.gitignore`.
- The database file `fitbuddy.db` is created automatically on first run.

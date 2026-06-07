**OpenForge** — Find beginner-friendly open-source issues quickly

**Live demo**
- Frontend (Netlify): https://open-forge.netlify.app/
- Backend (Render): https://openforge-48r0.onrender.com/

**One‑line pitch**
OpenForge indexes submitted GitHub repositories and surfaces “good first issue” items so new contributors can find easy, curated tasks fast.

**Why this matters**
- Saves time for new contributors looking for low-friction issues.  
- Curated, searchable project list with live GitHub issue lookups and short caching to avoid rate-limit spikes.

**Tech stack**
- Frontend: Static HTML/CSS/Vanilla JS (no build step).  
- Backend: Flask + requests.  
- Tests: Python unittest (backend).

**Quick local run (30s)**
1. Backend
```bash
cd backend
python -m venv .venv; source .venv/Scripts/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
# dev server (not for production)
python app.py
```
Or run a production-like server:
```bash
gunicorn backend.app:app --bind 0.0.0.0:5000
```

2. Frontend
```bash
cd frontend
# (optional) generate runtime config if you want to point at a different API URL
API_URL=http://127.0.0.1:5000/api bash ../scripts/generate-config.sh
python -m http.server 5500
# open http://127.0.0.1:5500/index.html
```

**Tests**
```bash
python -m unittest backend.test_app -v
```

**Production / deployment notes**
- Backend (Render): start with `gunicorn backend.app:app --bind 0.0.0.0:$PORT`. Set `GITHUB_TOKEN` in Render environment (do NOT commit it).  
- Frontend (Netlify): `netlify.toml` and `scripts/generate-config.sh` generate `frontend/config.js` at build time from `API_URL` environment variable.
- CORS: backend supports restricting origins via `ALLOWED_ORIGIN` env var.

**Security & housekeeping**
- Do NOT commit `backend/.env`. Rotate any tokens that were stored there.  
- Dynamic content is rendered with safe DOM APIs (no unescaped innerHTML for user-supplied text).

**Known limitations (short)**
- Persistence is a single JSON file (`backend/data.json`) — acceptable for demo but not horizontally scalable or durable.  
- Issue lookups are cached in-memory for 15 minutes (see `ISSUE_CACHE_TTL_SECONDS` in `backend/app.py`).  
- No authentication or rate-limiting on POST — suitable for a hackathon demo but not production hardened.

**What to evaluate (for judges)**
- Correctness of API endpoints: `GET /api/projects`, `POST /api/projects`, `GET /api/issues`.  
- UX: discoverability, search, and live issues behavior under load.  
- Tests: run `backend/test_app.py` to verify behavior covered by unit tests.

**Contribute / Contact**
- Open an issue or PR on the repository. License: MIT.

---
Updated for the hackathon submission to include live demo links and deployment instructions.

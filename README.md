# InternPilot

InternPilot is a role-aware career intelligence assistant that helps users with:
- Resume Match
- Recruiter View
- Interview Evaluation
- Model Resume generation

It supports both tech and non-tech roles (for example: web developer, corporate lawyer, HR, finance, marketing).

## Tech Stack

- Backend: FastAPI (Python)
- Frontend: React + Vite
- Storage:
  - Free deploy mode: browser `localStorage` for history persistence on refresh
  - Optional backend history API via SQLite

## Main Features

- Role-aware skill matching from JD + resume
- Recruiter-style decision, missing skills, and roadmap
- Interview mock question generation + answer scoring
- JD-aligned model resume generation (`.doc` download)
- Search history sidebar with delete/clear
- History survives refresh in deployed app (same browser/device)

## Project Structure

```text
internpilot/
  main.py
  services/
    ai_service.py
  utils/
  frontend/
    src/
```

## Local Setup

### 1) Backend

```bash
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
uvicorn main:app --reload
```

Backend default URL: `http://127.0.0.1:8000`

### 2) Frontend

```bash
cd frontend
npm install
npm run dev
```

Frontend default URL: `http://127.0.0.1:5173`

## Environment Variables

### Backend (`main.py`)

- `CORS_ORIGINS` (comma-separated list)
- `SMTP_HOST`
- `SMTP_PORT`
- `SMTP_USER`
- `SMTP_PASS`
- `SMTP_FROM`
- `HISTORY_DB_PATH` (optional)

### Frontend (Vite)

- `VITE_API_BASE` (example: `https://your-backend.onrender.com`)
- `VITE_GOOGLE_CLIENT_ID` (optional, for Google login in resume download flow)

## Deployment

## Backend (Render)

1. Connect GitHub repo to Render Web Service
2. Build command:
   - `pip install -r requirements.txt`
3. Start command:
   - `uvicorn main:app --host 0.0.0.0 --port $PORT`
4. Add required environment variables in Render
5. Deploy latest commit

## Frontend (Vercel or Render Static)

1. Set `VITE_API_BASE` to your backend URL
2. Build command: `npm run build`
3. Output directory: `dist`
4. Deploy and hard refresh browser once

## Git Workflow

```bash
git add .
git commit -m "Your update message"
git push origin main
```

Render: `Manual Deploy` -> `Deploy latest commit`

## Notes on Search History

- On free hosting, persistent server disk may not be available.
- InternPilot keeps search history in browser storage, so history remains after refresh/reopen on the same browser/device.
- If browser storage is cleared, history is removed.

## API Endpoints (Core)

- `POST /upload-and-analyze`
- `POST /evaluate-answer`
- `POST /extract-resume-links`
- `POST /generate-resume-reference`
- `GET /history`
- `POST /history/add`
- `DELETE /history/{item_id}`
- `DELETE /history/clear`

## Troubleshooting

- If frontend cannot reach backend:
  - verify `VITE_API_BASE`
  - verify CORS in `CORS_ORIGINS`
- If uploaded PDF returns low-quality result:
  - ensure PDF has selectable text (not only scanned image)


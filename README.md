# LifeOS (MVP)

## Run
```bash
docker compose up -d                      # PostgreSQL
sudo apt install tesseract-ocr            # OCR engine (macOS: brew install tesseract)

cd backend && python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env                      # set SECRET_KEY, optionally ANTHROPIC_API_KEY
set -a && source .env && set +a
uvicorn app.main:app --reload             # http://localhost:8000/docs

# frontend: create the Next.js shell, then copy these files over it
npx create-next-app@latest frontend-app --ts --tailwind --app --no-src-dir --import-alias "@/*"
cp -r frontend/app frontend/lib frontend-app/ && cd frontend-app && npm run dev
```
Without ANTHROPIC_API_KEY everything still works using rule-based fallbacks.

## Design rules built in
- Receipts are saved as `pending_review` and only count after you confirm.
- Budget numbers carry a `certainty` label (confirmed / estimate).
- Recurring expenses are suggestions only; the app never creates anything for you.
- The AI only sees your structured data and is told not to invent items or claim actions.

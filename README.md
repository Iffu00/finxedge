# finxedge
FinXEdge - AI Wealth &amp; Crypto Insights (MVP)
# FinXEdge — MVP

This repository contains a simple MVP for FinXEdge:
- `backend/` — Node.js + Express backend (market worker, CoinDCX connector, Redis)
- `frontend/` — Next.js frontend (demo dashboard + admin backtest)
- Use Render for backend and Vercel for frontend (both support GitHub import)

To deploy (phone-friendly):
1. Import this repository to Vercel and set project root = `frontend`.
2. Create a Web Service on Render, use repository root and set service root = `backend`.
3. Add environment variables: REDIS_URL, DATABASE_URL (optional), JWT_SECRET, NEXT_PUBLIC_API_BASE (backend URL).

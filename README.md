# finxedge
FinXEdge - AI Wealth &amp; Crypto Insights (MVP)
export default async function handler(req, res){
  const BACKEND = process.env.NEXT_PUBLIC_API_BASE || 'http://localhost:4000'
  const r = await fetch(`${BACKEND}/api/admin/backtest`, { method: 'POST', headers: {'Content-Type':'application/json'}, body: JSON.stringify(req.body || {}) })
  const j = await r.json()
  res.status(200).json(j)
}

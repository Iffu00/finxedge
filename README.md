# finxedge
FinXEdge - AI Wealth &amp; Crypto Insights (MVP)
// Proxy to backend (assumes backend at http://localhost:4000 or deployed backend URL)
export default async function handler(req, res){
  const BACKEND = process.env.NEXT_PUBLIC_API_BASE || 'http://localhost:4000'
  const r = await fetch(`${BACKEND}/api/signals/latest`)
  const j = await r.json()
  res.status(200).json(j)
}

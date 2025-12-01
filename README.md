# finxedge
FinXEdge - AI Wealth &amp; Crypto Insights (MVP)
import { useState } from 'react'
export default function AdminSignals(){
  const [res, setRes] = useState(null)
  const run = async ()=>{
    const r = await fetch('/api/admin/backtest',{method:'POST', headers:{'Content-Type':'application/json'}, body: JSON.stringify({})})
    const j = await r.json()
    setRes(j)
  }
  return (
    <div style={{padding:24}}>
      <h2>Admin — Signals & Backtest</h2>
      <p>Click the button below to run a quick demo backtest (uses synthetic data).</p>
      <button onClick={run} style={{padding:'10px 16px', borderRadius:8, background:'#0ea5a4', color:'#fff', border:0}}>Run Backtest</button>
      <pre style={{marginTop:12, background:'#f6f8fa', padding:12, borderRadius:8}}>{JSON.stringify(res, null, 2)}</pre>
    </div>
  )
}

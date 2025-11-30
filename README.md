# finxedge
FinXEdge - AI Wealth &amp; Crypto Insights (MVP)
import useSWR from 'swr'
const fetcher = (url)=>fetch(url).then(r=>r.json())
export default function Home(){
  const { data } = useSWR('/api/signals/latest', fetcher, { refreshInterval: 3000 })
  return (
    <div style={{fontFamily:'Inter, sans-serif', padding:24}}>
      <h1 style={{marginBottom:8}}>FinXEdge — Demo Dashboard</h1>
      <p>Live signal (BTC/INR):</p>
      <pre style={{background:'#f6f8fa', padding:12, borderRadius:8}}>{JSON.stringify(data, null, 2)}</pre>
      <p><a href="/admin/signals">Admin Signals</a></p>
    </div>
  )
}

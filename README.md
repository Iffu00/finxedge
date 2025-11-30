# finxedge
FinXEdge - AI Wealth &amp; Crypto Insights (MVP)
// Simple SMA crossover engine
class SignalEngine {
  constructor(){
    this.prices = []
    this.lastSignal = null
  }
  pushTick(tick){
    if (!tick || typeof tick.price !== 'number') return null
    this.prices.push({ts: tick.ts, price: tick.price})
    if (this.prices.length > 500) this.prices.shift()
    const prices = this.prices.map(p=>p.price)
    const sma = (n)=>{
      if (prices.length < n) return null
      const slice = prices.slice(prices.length - n)
      return slice.reduce((a,b)=>a+b,0)/slice.length
    }
    const s = sma(5)
    const l = sma(20)
    if (s===null || l===null) return null
    if (s > l && this.lastSignal !== 'BUY'){
      this.lastSignal = 'BUY'
      return { symbol: 'BTC/INR', action: 'BUY', confidence: Math.round((s-l)/l*100) }
    }
    if (s < l && this.lastSignal !== 'SELL'){
      this.lastSignal = 'SELL'
      return { symbol: 'BTC/INR', action: 'SELL', confidence: Math.round((l-s)/l*100) }
    }
    return null
  }
}
module.exports = SignalEngine

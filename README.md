# finxedge
FinXEdge - AI Wealth &amp; Crypto Insights (MVP)
const WebSocket = require('ws')
const EventEmitter = require('events')

class CoinDCXClient extends EventEmitter {
  constructor() {
    super()
    this.url = 'wss://stream.coindcx.com/market'
    this.ws = null
    this.pairs = ['B-BTC_INR']
  }

  connect() {
    this.ws = new WebSocket(this.url)
    this.ws.on('open', () => {
      const msg = { type: 'subscribe', channels: [{ name: 'ticker', instruments: this.pairs }] }
      try { this.ws.send(JSON.stringify(msg)) } catch (e) {}
      console.log('CoinDCX WS connected')
    })
    this.ws.on('message', (raw) => {
      try {
        const data = JSON.parse(raw)
        if (data && data.type === 'ticker') {
          const tick = {
            symbol: data.instrument || 'BTC/INR',
            price: parseFloat(data.last_price || data.l || data.ls || 0),
            ts: Date.now()
          }
          this.emit('tick', tick)
        }
      } catch (e) { /* ignore */ }
    })
    this.ws.on('close', () => setTimeout(()=>this.connect(), 2000))
    this.ws.on('error', (e) => console.error('CoinDCX WS error', e))
  }
}

module.exports = CoinDCXClient

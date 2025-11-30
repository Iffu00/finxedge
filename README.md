# finxedge
FinXEdge - AI Wealth &amp; Crypto Insights (MVP)
const express = require('express')
const bodyParser = require('body-parser')
const cors = require('cors')
const jwt = require('jsonwebtoken')
const redis = require('./cache/redis-client')
const CoinDCXClient = require('./integrations/coindcx-ws')
const SignalEngine = require('./signal-engine')

const app = express()
app.use(cors())
app.use(bodyParser.json())

const JWT_SECRET = process.env.JWT_SECRET || 'dev'

app.get('/health', (req, res) => res.json({ status: 'ok' }))

app.get('/api/market/stream', async (req, res) => {
  res.setHeader('Content-Type', 'text/event-stream')
  res.setHeader('Cache-Control', 'no-cache')
  res.setHeader('Connection', 'keep-alive')
  const sub = require('ioredis')(process.env.REDIS_URL)
  sub.subscribe('market_updates')
  sub.on('message', (_, message) => {
    res.write(`data: ${message}\n\n`)
  })
  req.on('close', () => sub.disconnect())
})

app.get('/api/signals/latest', async (req, res) => {
  const s = await redis.get('signals:latest:BTC_INR')
  res.json({ signal: s ? JSON.parse(s) : null })
})

const client = new CoinDCXClient()
const engine = new SignalEngine()

client.on('tick', async (tick) => {
  await redis.set('market:BTC_INR:latest', JSON.stringify(tick))
  const sig = engine.pushTick(tick)
  if (sig) {
    await redis.set('signals:latest:BTC_INR', JSON.stringify(sig))
    await redis.publish('market_updates', JSON.stringify({ tick, sig }))
  } else {
    await redis.publish('market_updates', JSON.stringify({ tick }))
  }
})

client.connect()

const PORT = process.env.PORT || 4000
app.listen(PORT, () => console.log('Backend running on port', PORT))

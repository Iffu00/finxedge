# finxedge
FinXEdge - AI Wealth &amp; Crypto Insights (MVP)
const Redis = require('ioredis')
const redis = new Redis(process.env.REDIS_URL || 'redis://127.0.0.1:6379')
module.exports = redis

# 🌟 StellarMonitor

> **Real-time on-chain intelligence platform for the Stellar Network**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Python](https://img.shields.io/badge/Python-3.11+-blue)](https://python.org)
[![Stellar](https://img.shields.io/badge/Network-Stellar-black)](https://stellar.org)
[![Live](https://img.shields.io/badge/Status-Live-green)](https://stellarmonitor.xyz)

**🌐 [stellarmonitor.xyz](https://stellarmonitor.xyz) · 📱 [Telegram](https://t.me/stellarmonitor) · 🐦 [Twitter/X](https://x.com/StellarMonitor)**

---

## What is StellarMonitor?

StellarMonitor is an open-source on-chain intelligence platform for the Stellar Network. It monitors whale transfers, SDEX activity, Soroban smart contract events, and liquidity pools — delivering real-time alerts via Telegram, Twitter/X, and a public web dashboard.

---

## Platform Features

### 📡 Live Feed
Real-time alert stream with whale transfers, SDEX trades, airdrops, and Soroban events. Auto-refreshes every 2 seconds. Filterable by type and USD value.

### 🔍 Wallet Scanner
Deep transaction history explorer for any Stellar address.
- Up to 10,000 pages of transaction history
- Filter by type, asset, period, USD value
- Exchange detection (TOML-verified + 30+ known addresses)
- Balance history area chart with daily net flow
- Activity stats: inflow, outflow, net flow, counterparties, exchange txs
- Sort by Newest / Oldest / Largest
- Hover on timestamps for exact dates

### 🕸️ Wallet Graph
Interactive force-directed node graph visualizing transaction connections.
- Depth 1/2/3 exploration hops
- Node type filters: Root / Sent to / Received from / Both / Exchange
- Auto-fit zoom for large graphs
- Click node → load its graph · Double-click → open in Scanner
- Balance history panel with area chart per period (1D/1W/1M/1Y/All)

### 💧 Liquidity Pools
Live AMM pool rankings sourced from Stellar Expert API.
- 1000+ pools ranked by TVL, Volume, APR, Earned fees
- Earned 24h column: real fee revenue for LPs in USD
- Filter by minimum volume, sort by any column
- Add Liquidity links for active pools

---

## Architecture
```
Stellar Horizon API (SSE Stream)
        │
        ▼
┌─────────────────────┐
│   Monitor Bot       │  ← Python 3.11, asyncio
│   (main.py)         │  ← Whale, SDEX, Soroban, Airdrop detectors
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│   Publisher         │  ← Telegram Bot API + Twitter/X API v2
│                     │  ← SQLite deduplication
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│   FastAPI           │  ← REST API at api.stellarmonitor.xyz
│   (api.py)          │  ← Rate limited, CORS restricted
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│   Web Dashboard     │  ← HTML/CSS/JS, hosted on VPS
│   stellarmonitor.xyz│  ← Reads Horizon + Stellar Expert + own API
└─────────────────────┘
```

---

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Bot | Python 3.11, asyncio, Tweepy, python-telegram-bot |
| API | FastAPI, SQLite, Uvicorn |
| Frontend | Vanilla HTML/CSS/JS (no framework) |
| Data | Stellar Horizon SSE, Stellar Expert API, CoinGecko |
| Infra | Ubuntu VPS, Nginx, Certbot SSL, systemd |

---

## API

Base URL: `https://api.stellarmonitor.xyz`
```
GET /api/alerts?limit=100&type=WHALE_USDC&min_usd=50000
GET /api/stats
GET /api/whale?min_usd=500000&limit=20
GET /api/pairs?limit=5
```

No authentication required. Rate limited to 60 requests/minute per IP.

---

## Self-hosting
```bash
git clone https://github.com/withercryptodev-star/stellarmonitor
cd stellarmonitor

# Install Python dependencies
pip install -r bot/requirements.txt

# Configure
cp bot/settings.env.example bot/settings.env
# Edit settings.env with your API keys

# Run the bot
python bot/main.py

# Run the API
uvicorn bot/api:app --host 127.0.0.1 --port 8000
```

### Required environment variables
```env
TWITTER_API_KEY=
TWITTER_API_SECRET=
TWITTER_ACCESS_TOKEN=
TWITTER_ACCESS_SECRET=
TELEGRAM_BOT_TOKEN=
TELEGRAM_CHAT_ID=
HORIZON_URL=https://horizon.stellar.org
WHALE_THRESHOLD_XLM=1000000
WHALE_THRESHOLD_USDC=50000
```

---

## Security

- All traffic over HTTPS (Let's Encrypt)
- Security headers: HSTS, CSP, X-Frame-Options, X-Content-Type-Options
- API rate limiting (Nginx + FastAPI middleware)
- CORS restricted to stellarmonitor.xyz
- No cookies, no tracking, no user data stored

---

## License

MIT — free to use, fork, and build on.

---

*No whale goes undetected.*

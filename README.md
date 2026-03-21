# 🌟 StellarMonitor

> **Real-time on-chain intelligence bot for the Stellar Network — surfacing whale moves, DEX activity, and Soroban events directly to Twitter/X.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Python](https://img.shields.io/badge/Python-3.11+-blue)](https://python.org)
[![Stellar](https://img.shields.io/badge/Network-Stellar-black)](https://stellar.org)
[![Soroban](https://img.shields.io/badge/Smart%20Contracts-Soroban-purple)](https://soroban.stellar.org)
[![Status](https://img.shields.io/badge/Status-In%20Development-yellow)]()

---

## What is StellarMonitor?

StellarPulse is an open-source monitoring bot that watches the Stellar blockchain in real time and automatically posts notable on-chain activity to a public Twitter/X account.

Most blockchain activity goes unnoticed. Block explorers exist, but they require you to already know what to look for. StellarPulse flips this — it watches everything and surfaces what matters, in plain English, to anyone following the account.

**Live Twitter feed:** [@StellarMonitor](https://x.com/StellarMonitor)

---

## What it tracks

| Event Type | Description |
|---|---|
| 🐋 **Whale Transfers** | Large XLM / USDC / asset movements above configurable thresholds |
| 📊 **SDEX Activity** | Big order fills and unusual trading patterns on Stellar DEX |
| 💧 **Liquidity Pools** | Significant LP deposits, withdrawals, and pool creation events |
| 🤖 **Soroban Contracts** | Smart contract invocations and notable contract events |
| 🔍 **Wash Trading** | Suspicious circular transaction patterns between wallets |
| 🌐 **Cross-border Payments** | Large cross-border payment flows via Stellar anchors |

---

## Architecture

```
Stellar Horizon API (SSE Stream)
        │
        ▼
┌─────────────────────┐
│   Event Ingestion   │  ← Horizon SSE + Soroban RPC
│   & Normalization   │
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│   Detection Engine  │  ← Threshold rules + pattern matching
│   (Rules + Filters) │
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│   Formatter         │  ← Human-readable tweet generation
│   & Enrichment      │  ← StellarExpert entity labels
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│   Twitter/X API v2  │  ← Rate-limited posting pipeline
│   Publishing Layer  │
└─────────────────────┘
```

---

## Tech Stack

- **Python 3.11+** — async event processing, ThreadPoolExecutor
- **Stellar Horizon API** — real-time transaction streaming via SSE
- **Soroban RPC** — smart contract event monitoring
- **StellarExpert API** — address labeling (exchanges, anchors, market makers)
- **Twitter/X API v2** — automated tweet publishing with thread support
- **SQLite** — local deduplication and event history

---

## Quickstart

```bash
# Clone the repo
git clone https://github.com/YOUR_HANDLE/stellarpulse
cd stellarpulse

# Install dependencies
pip install -r requirements.txt

# Configure environment
cp .env.example .env
# Edit .env with your API keys

# Run the bot
python main.py
```

### Environment variables

```env
# Twitter/X API
TWITTER_API_KEY=
TWITTER_API_SECRET=
TWITTER_ACCESS_TOKEN=
TWITTER_ACCESS_SECRET=

# Stellar
HORIZON_URL=https://horizon.stellar.org
SOROBAN_RPC_URL=https://soroban-testnet.stellar.org

# Thresholds
WHALE_THRESHOLD_XLM=1000000
WHALE_THRESHOLD_USDC=50000
```

---

## Configuration

Edit `config.yaml` to customize what gets monitored and posted:

```yaml
alerts:
  whale_transfers:
    enabled: true
    min_xlm: 1_000_000
    min_usdc: 50_000

  sdex:
    enabled: true
    min_fill_usdc: 10_000

  soroban:
    enabled: true
    track_contracts: []   # empty = all contracts

  liquidity_pools:
    enabled: true
    min_change_usdc: 25_000

posting:
  max_tweets_per_hour: 12
  include_explorer_link: true
  explorer_base: "https://stellar.expert/explorer/public"
```

---

## Example Tweets

> 🐋 **Whale Alert** — Stellar Network  
> `GBXXX...` just moved **4,200,000 XLM** (~$630K)  
> To: `GCYYY...` (Binance Hot Wallet)  
> 🔗 stellar.expert/tx/...

---

> 📊 **SDEX Large Fill**  
> **12,400 XLM/USDC** filled in a single order  
> Price: 0.1502 | Spread: 0.02%  
> 🔗 stellar.expert/tx/...

---

> 🤖 **Soroban Event**  
> Liquidity +$890,000 added to XLM/USDC pool  
> Contract: `CABC...` (Verified AMM)  
> 🔗 stellar.expert/tx/...

---

## Roadmap

- [x] Project architecture design
- [x] Horizon SSE integration research
- [ ] Core event ingestion pipeline
- [ ] Detection engine (whale + SDEX rules)
- [ ] Twitter/X publishing layer
- [ ] Soroban contract event support
- [ ] Address enrichment via StellarExpert
- [ ] Web dashboard (public feed)
- [ ] Telegram bot variant
- [ ] Open API for third-party integrations

---

## Contributing

Contributions welcome! Please open an issue first to discuss what you'd like to change.

```bash
git checkout -b feature/your-feature
# make changes
git commit -m "feat: your feature description"
git push origin feature/your-feature
# open Pull Request
```

---

## License

MIT — free to use, fork, and build on.

---

## Built with support from

<img src="https://stellar.org/images/stellar-logo.svg" height="24" alt="Stellar"> &nbsp; **Stellar Community Fund (SCF)**

---

*StellarPulse makes the Stellar blockchain visible — one tweet at a time.*

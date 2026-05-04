# Earnings Radar PRO — Project Memory

## Project Overview
A personal trading dashboard built around earnings calls as the primary decision trigger.
Hosted at: https://tarreola.github.io/earnings-radar/
GitHub repo: https://github.com/tarreola/earnings-radar
File: single `index.html` — no framework, no build process

---

## The Trader's Strategy
- Earnings calls are the core decision pillar
- Analyze stock behavior 1 day after + 1 week before earnings
- Study last 6–10 earnings calls per stock to detect patterns
- Search for external signals (analyst consensus, key metrics)
- Focus: Mag7 + big tech + recognized names
- Look ahead 2 weeks (now extended to 4 weeks) for upcoming calls

---

## Tech Stack
| Layer | Technology |
|---|---|
| Frontend | Single `index.html` — vanilla JS |
| Live prices | Finnhub WebSocket (145 stocks) |
| AI engine | Claude API — `claude-sonnet-4-6` + web search |
| Local storage | IndexedDB (instant, offline-safe) |
| Cloud storage | Supabase (permanent, cross-device, sync when connected) |
| Export | CSV download |
| Hosting | GitHub Pages |
| Fonts | Bebas Neue · JetBrains Mono · IBM Plex Sans |

---

## API Keys (user configures in Settings — never hardcoded)
1. **Finnhub** — finnhub.io — live prices via WebSocket
2. **Anthropic** — console.anthropic.com — AI analysis
3. **Supabase** — supabase.com — cloud trade journal storage

---

## Design Language (Round 6 decisions)
| Element | Decision |
|---|---|
| Theme | Bloomberg dark terminal — deep black |
| Background | Pure black `#000000` |
| Primary accent | Amber gold `#d4a017` |
| Green (bullish) | `#10b981` — balanced |
| Red (bearish) | `#ef4444` — balanced |
| Blue (neutral) | `#38bdf8` |
| Corner radius | 6–8px — professional |
| Header | Medium weight — logo + market status + alert banner |
| Dividers | Slightly visible lines with glow |
| Number font | JetBrains Mono |
| Display font | Bebas Neue |
| Body font | IBM Plex Sans |
| Animations | Smooth — satisfying transitions |
| Price ticks | Flash green/red briefly, return to normal |
| AI loading | Pulsing dot animation |
| Trade logged | Card flies into journal |
| Light mode | True white `#ffffff` — Apple style |

---

## Layout — Single Page, No Scroll
```
HEADER — Logo · Market Status · Journal btn · Compare btn · Settings
ALERT BANNER — stacked pills for stocks within 7 days
────────────────────────────────────────────────────────
SIDEBAR          │ AI ANALYSIS  │ PRICE CHART  │ RESULTS TABLE
(260px)          │ (200px)      │ (flex)       │ (220px) + SIGNAL
• Search bar     │ 5 bullets    │ SVG line     │ 4-col table
• Upcoming 4wk   │ News drawer  │ earnings     │ Load 5 more
• Watchlist      │              │ dots + zone  │ avg footer
  groups         │              │ 3M/6M/12M/ALL│ SIGNAL PANEL
```

---

## Watchlist — 145 Stocks Across 11 Groups

### Groups
- ★ Favorites (user-starred, pinned top)
- Mag 7: AAPL, MSFT, NVDA, TSLA, GOOG, AMZN, META
- Semis + Hardware: AMD, INTC, DELL, HPE, HPQ
- SaaS + Cloud: ADBE, CRM, ORCL, SNOW, CRWD, PLTR, WDAY, MNDY, ADSK, SHOP, UPWK
- Fintech + Payments: COIN, HOOD, PYPL, V, MA, AXP, NU, MELI, BULL, RKT
- Streaming + Media: NFLX, SPOT, WBD, RDDT, RBLX, GME
- Consumer + Retail: KO, PEP, MCD, SBUX, COST, WMT, TGT, HD, NKE, LULU, CROX
- Travel + Leisure: ABNB, EXPE, MAR, BKNG, DAL, AAL, RCL, RACE
- Defense + Space: RTX, KTOS, AVAV, RKLB, BA, MP, ASTS
- Quantum + Speculative: IONQ, QBTS, RGTI, CIFR, IREN, BMNR, RR, OKLO
- Global ADRs 🌍: RYAAY, TM, SAN, BABA, DB, NU, ING, MNSO

### All 145 Tickers
AAPL, MSFT, NVDA, TSLA, GOOG, AMZN, META, AMD, INTC, DELL, HPE, HPQ,
ADBE, CRM, ORCL, SNOW, CRWD, PLTR, WDAY, MNDY, ADSK, SHOP, UPWK, VMEO,
COIN, HOOD, PYPL, V, MA, AXP, NU, MELI, BULL, RKT, NFLX, SPOT, WBD,
RDDT, RBLX, GME, KO, PEP, MDLZ, HSY, DNUT, MCD, SBUX, COST, WMT, TGT,
HD, NKE, LULU, ONON, ETSY, URBN, BBY, CROX, BIRK, ABNB, EXPE, MAR,
BKNG, DAL, AAL, RCL, RACE, RYAAY, SHAK, JACK, F, TM, UBER, LYFT, FDX,
UPS, CAR, BAC, GS, MS, C, BLK, SAN, DB, ING, WU, KFY, PFE, LLY, OSCR,
HIMS, GRAB, TEM, RTX, KTOS, AVAV, RKLB, BA, MP, ASTS, IONQ, QBTS, RGTI,
CIFR, IREN, BMNR, RR, OKLO, SNAP, DUOL, BABA, BB, GDDY, APP, CSCO,
EBAY, TMUS, T, VZ, PINS, GRMN, RS, MNSO, BYND, GRPN, ISPO, MAT, YETI,
WTW, CAT, COUR, MET, KMB, JNJ, MMM, GT, FOX, NDAQ, M, DBX, LEU, ONDS, FL

### International Stocks (🌍 flag + different calendar note)
RYAAY, TM, SAN, BABA, DB, NU, ING, MNSO

---

## Feature Specs

### Sidebar — Each Row Shows
`TICKER  $price  ±%  ● signal dot  Xd  [sparkline]`
- Unanalyzed: greyed out + "tap to analyze" hint
- Default: Favorites expanded, all other groups collapsed
- Sort within group: nearest earnings date first
- Group state persisted to localStorage

### Upcoming Earnings Panel
- Confirmed dates only (Finnhub REST API)
- 4-week lookahead default
- Always shows BMO / AMC
- Signal dot visible before clicking
- Clicking auto-loads ticker into dashboard

### Alert Banner (below header)
- Stacked pills for ALL stocks with earnings ≤7 days
- Stays visible all day until earnings happen
- Tap pill → highlights in upcoming list (doesn't auto-load)
- Format: `⚡ AAPL 6d · est $1.62`

### Price Chart (center)
- SVG price line — real OHLC via Finnhub REST (fallback: simulated)
- Time ranges: 3M / 6M / 12M / ALL — default 12M
- Earnings dots: 🟢 beat+up · 🔴 miss · 🟡 beat+down · % label above dot
- Pre-earnings zone: 7-day shaded band, color-coded by drift direction
  - Majority positive drift → green zone
  - Majority negative drift → red zone
  - Mixed (tied) → amber zone
- Dot tap → tooltip (EPS, beat/miss, % move) → tap again → jumps to results table
- Time range zoom → AI bullets filter to visible quarters

### AI Analysis Panel (left)
- Tone: conversational, natural language
- 5 bullets per stock, updates on ticker change
- Filters to quarters visible in current chart range
- Engine: Claude API + web_search tool (requires anthropic-beta header)
- Cache: until earnings happen, then auto-refresh
- Auto-refresh: stocks within 7 days if stale > 4 hours
- 📡 News button → slide-in drawer → live web search

### Earnings History Table (right top)
- 4 columns: Quarter · Beat/Miss · Day After % · Week Before %
- 5 shown first → "Load 5 more" button
- Average footer row (not header)
- ⚠ flag if fewer than 10 calls available
- Notes field per row (tap to expand)
- Incomplete data: show with warning, don't hide

### Trade Signal Panel (right bottom)
Layout order:
1. Signal badge (Strong Buy Before / Mild Buy Before / Hold / Avoid)
2. Confidence meter + one-line reason (e.g. "84% · 9/9 beats, +8.1% avg")
3. 3 stat cards: Beat Rate · Avg Move · Pre-Drift
4. Rationale text (2–3 conversational sentences)
5. Exit Strategy (collapsed, tap to expand)
6. Signal Accuracy History (collapsed, tap to expand)
7. Log Trade + Refresh + Copy buttons
8. Disclaimer

**Urgency warning**: ≤2 days to earnings + BUY signal → "⚡ Position window closing"

**Post-earnings state**:
- Outcome card: signal correct? actual EPS, day-after move
- Next cycle prep: next earnings date, early signal

### Compare Mode
- Header ⇄ button → split view
- Two stocks side by side with dropdowns
- Shows: signal, confidence, beat rate, avg move, top 3 bullets
- Implemented via renderCompareView()

---

## Trade Journal

### Access
- Separate full-screen page via "📒 Journal" header button (desktop)
- Bottom tab bar on mobile (Dashboard · Journal · Settings)

### Log Entry — Buy Fields
- Ticker, Signal Used, Date Bought, Buy Price, Shares, Amount (auto-calc), Notes

### Log Entry — Sell Fields
- Date Sold, Sell Price, P&L Method (FIFO or Average Cost — choose per trade)
- P&L auto-calculated

### Auto-prompt
- Day after earnings → banner: "TICKER reported — ready to log your exit?"

### Trade Types
- Stocks only (shares bought and sold)

### Success Definition
- Stock moved UP AND trader made a profit = successful signal

### Card Layout
- One card per trade, newest first
- Green border = Win, Red = Loss, Gold = Open
- Open trades show live unrealized P&L via Finnhub price
- No filters — chronological order
- Search by ticker input at top

### Summary Stats (top of journal)
Total P&L · Win Rate · Avg Return · AI Accuracy % · Best Trade · Worst Trade

### Safety
- Delete trade: confirm dialog first
- Watchlist remove: trades stay in journal forever (fully separate)

### Storage
- IndexedDB: instant save, always works offline
- Supabase: sync when connected, queue when not
- Pending sync shown as ⏳ badge
- CSV export always available

### Supabase Table SQL
```sql
CREATE TABLE trades (
  id TEXT PRIMARY KEY,
  ticker TEXT, signal TEXT,
  "buyDate" TEXT, "buyPrice" REAL,
  shares REAL, amount REAL,
  "sellDate" TEXT, "sellPrice" REAL,
  pnl REAL, pct REAL,
  status TEXT, notes TEXT,
  "pendingSync" BOOLEAN
);
```

---

## Market Status Indicator (header, always visible)
```
🔵 PRE-MARKET     4:00 AM – 9:30 AM ET
🟢 MARKET OPEN    9:30 AM – 4:00 PM ET
🟠 AFTER-HOURS    4:00 PM – 8:00 PM ET
🔴 MARKET CLOSED  8:00 PM – 4:00 AM ET
```
- Uses `toLocaleDateString('en-CA', { timeZone: 'America/New_York' })` for correct ET date
- US holidays hardcoded for 2025–2026
- Updates every 30 seconds

---

## Settings Panel
- Gear icon → slide-in from right
- API Keys: Finnhub, Anthropic, Supabase key + URL
- Theme: Dark / Light toggle
- Default time range selector
- Watchlist group manager + Add Group
- Export CSV button
- Sync to Supabase button + SQL setup instructions

---

## Mobile (iPhone)
- First panel: Upcoming earnings list
- Bottom tab bar: Dashboard · Journal · Settings
- ☰ hamburger shows on mobile to open sidebar
- Chart: full width, compact height — chart above, stats below
- AI + Signal panels visible, stacked below chart
- All tap targets ≥ 44px
- News in slide-in drawer

---

## Known Issues Fixed (26 total — all applied to current index.html)

| # | Issue | Status |
|---|---|---|
| 1 | Missing `anthropic-beta: web-search-2025-03-05` header | ✅ Fixed |
| 2 | Market status timezone bug (UTC vs ET) | ✅ Fixed |
| 3 | No desktop journal navigation | ✅ Fixed |
| 4 | Compare mode not implemented | ✅ Fixed |
| 5 | localStorage cache 5MB limit | ✅ Fixed (capped at 20) |
| 6 | Chart using random data not real OHLC | ✅ Fixed (fetchOHLC added) |
| 7 | No mobile sidebar toggle button | ✅ Fixed |
| 8 | Signal panel overflow on small screens | ✅ Fixed |
| 9 | Demo mode updating all 145 tickers every 3s | ✅ Fixed (visible only) |
| 10 | Trade card fly-in animation spamming on every render | ✅ Fixed |
| 11 | expandedGroups Set not persisted across sessions | ✅ Fixed |
| 12 | Search no-match check always found match in 'all' group | ✅ Fixed |
| 13 | Fragile inline onclick in signal panel buttons | ✅ Fixed (data attributes) |
| 14 | No onboarding banner on first open | ✅ Fixed |
| 15 | Mobile AI + Signal panels completely hidden | ✅ Fixed |
| 16 | Journal search missing | ✅ Fixed |
| 17 | Supabase table not created guidance | ✅ Fixed |
| 18 | Close trade chain from dashboard untested | ✅ Fixed |
| 19 | Missing `x-api-key` header in both Anthropic fetch calls (401 on every AI call) | ✅ Fixed |
| 20 | News fetch didn't check `res.ok` — HTTP errors swallowed silently | ✅ Fixed |
| 21 | Finnhub WebSocket subscribed all 145 tickers (free tier limit is 50) | ✅ Fixed (capped at 50) |
| 22 | Outdated model `claude-sonnet-4-5` → `claude-sonnet-4-6` | ✅ Fixed |
| 23 | `toggleShowAll` button passed serialized JSON as 2nd arg (ignored + unsafe) | ✅ Fixed |
| 24 | `BLSH` invalid ticker → `BULL` (in both ALL_TICKERS and GROUPS.fintech) | ✅ Fixed |
| 25 | `renderJournalStats` mutated `closed` array with two in-place sorts | ✅ Fixed (spread copies) |
| 26 | `favorites` not declared in initial `S` state object | ✅ Fixed |

---

## Current Status
- ✅ Live at https://tarreola.github.io/earnings-radar/
- ✅ Demo mode working
- ✅ All 26 fixes applied to index.html
- ✅ Settings panel functional
- ✅ AI calls now authenticated (`x-api-key` header present)
- ✅ Model updated to `claude-sonnet-4-6`

---

## Workflow Going Forward
1. User is in Claude Code (`cd ~/earnings-radar && claude`)
2. Each fix = one prompt to Claude Code + one git commit
3. Fix → commit → push → test on live URL
4. Format: `git add index.html && git commit -m "Fix N: description" && git push`

---

## 7 Rounds of Design Decisions (86 total)
| Round | Topic | Count |
|---|---|---|
| 1 | Layout, stocks, chart, history | 12 |
| 2 | Color, density, zones, alerts, tone | 10 |
| 3 | Banner, zoom, signal logic, edge cases | 10 |
| 4 | Sidebar, upcoming, signal panel, results | 14 |
| 5 | Trade journal, logging, storage, view | 12 |
| 6 | UX, design language, motion, mobile | 15 |
| 7 | Gaps — caching, P&L, safety, errors, search | 13 |

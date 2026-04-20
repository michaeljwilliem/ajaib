# Ajaib Design Prototypes

This repo contains clickable HTML prototypes for the Ajaib investment app, used for internal design exploration and team reference.

## Live URL
GitHub Pages: https://michaeljwilliem.github.io/ajaib/

## Structure

```
ajaib/
├── desktop/
│   └── topmovers/
│       └── index.html   # Desktop stock list redesign
└── mobile/
    └── home/
        └── index.html   # Mobile home screen redesign
```

## Local dev server

Run from `/Users/liem/ajaib`:
```bash
python3 -m http.server 4444
```

- Desktop prototype: http://localhost:4444/desktop/topmovers/
- Mobile prototype:  http://localhost:4444/mobile/home/

## Desktop Prototype (`desktop/topmovers/`)

Redesigned stock list for the Ajaib desktop app (column 3, top section).

**Design goals:**
- Higher density — more tickers in same vertical space (32px rows)
- Mini sparkline charts per row
- IDX and US market session support

**Key features:**
- Dark theme (`#1f1f1f`), Gabarito font
- Session states: IDX (OPEN / PRE-OPENING / CLOSED), US (Regular / Pre-Market / After Market / Overnight)
- IDX pre-opening: amber CTA bar encouraging order placement; portfolio/order dots with hover tooltips in Indonesian
- US stocks: two-line price layout — session price + Reg. Close reference; full-word session labels (no abbreviations)
- Live price simulation (DOM in-place updates, flash animations)
- Labels: `10x`, `B`, `F` at readable size

**IDX rules:**
- Market closed: still show day's % and value change (traders analyze post-market)
- Pre-opening: no auction; show portfolio/order position indicators

## Mobile Prototype (`mobile/home/`)

Redesigned mobile home screen — optimized for watchlist density on first fold.

**Design goals:**
- 60–70% of first fold = watchlist
- Multi-asset watchlist (IDX, Crypto, US, Reksa Dana, Obligasi) in one blended list
- Full-width search bar visible (not icon)
- Total aset prominent with full Rp amount (no shortform — clarity is critical for an investment app)

**Layout (top → bottom):**
1. Status bar (iOS, 9:41)
2. App bar: Ajaib lamp logo + "ajaib" wordmark | bell + avatar
3. Total Aset: full `Rp X.XXX.XXX` (never abbreviated)
4. Info carousel (auto-rotates 3s): Buying Power IDX / US / Kripto / Posisi Terbuka + Deposit CTA + shortcut icons
5. Search bar: animated placeholder cycling `🪔 ajaib` → `Cari Saham IDX…` → `Cari BTC…` → etc.
6. Watchlist (column headers + filter pills + rows)
7. Recommendations (below fold, accessible via scroll)
8. Bottom tab bar (sticky)

**Watchlist column layout:** Aset | Grafik | Harga | Pergerakan

**Asset type chips + badges:**
- IDX: red chip + ⚡`Nx` thunder badge for leveraged stocks (amber)
- US: blue chip + `24H` badge (reminds users US trades 24/5)
- CRYPTO: orange chip
- RD: purple chip
- OBL: teal chip

**Price formatting rules:**
- IDX: `Rp X.XXX` (full Rupiah, no shortform in rows)
- US: `$XXX.XX`
- Crypto IDR pair: `Rp 1,07M` (miliar) / `Rp 56,0jt` (juta)
- Crypto USDT pair: `$X,XXX`
- RD: NAV `X.XXX /unit`
- OBL: coupon `X,XX% p.a.` + "Stabil · Bulanan"

**US extended hours:** two-line row (60px) — session price on top + Reg. Close dimmed below. Full-word labels: "After Market", "Pre-Market", "Overnight" (never abbreviations).

**Live simulation:** IDX, Crypto, US all tick every 1500ms with flash animations. US state switchable via the Controller panel.

**UX copy (Indonesian):**
- Edit list: "Ubah Urutan"
- Add to watchlist: "Tambahkan aset ke watchlist kamu"

**iOS phone frame:** 407px outer shell, 387×839px screen, Dynamic Island, side buttons, status bar, home indicator.

**Mobile fullscreen mode** (activates on `max-width: 500px` or `pointer: coarse`):
- Phone shell, side buttons, and drop-shadow are hidden; screen fills `100svh` edge-to-edge
- The Dynamic Island becomes a tappable "Controller ∨" pill — tap to show/hide the US market controls panel, which drops down from the top below the status bar; chevron rotates when open
- On desktop the prototype looks identical to before

## Design principles (from product owner)

- Clarity on amounts is non-negotiable — full Rp numbers, never abbreviated in primary contexts
- Indonesian users don't know US stocks trade 24/5 — always educate contextually
- IDX post-market data is still meaningful — show day change even when market is closed
- Buying power and deposit CTA should always be accessible without scrolling
- Watchlist is the primary feature — maximize its vertical real estate on first fold

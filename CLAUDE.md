# Ajaib Design Prototypes

This repo contains clickable HTML prototypes for the Ajaib investment app, used for internal design exploration and team reference.

## Figma sources

- Desktop stock list: https://www.figma.com/design/qBOOXR9BcEQRIyH1xB8QY7/-Desktop-App--ID-Stocks-Phase-1?node-id=11358-409390
- Mobile home screen: https://www.figma.com/design/FqZ8Rnk2nhYOLRFZ2z5IWP/Homepage-

## Handoff ritual

When the user says **"update .md"** or **"update CLAUDE.md"**:
1. Update the "Current state" block below — set today's date, write 1–2 sentences on what was just finished, update the known issues / open work checklist
2. Commit everything (all edited files + this CLAUDE.md) with a descriptive message summarising the session's work

## Current state

**Last worked on:** 2026-04-24  
**Active file:** `concepts/mobile/home/index.html`  
**What's done:** Fixed IHSG (and all indices) tap-to-sheet: ghost items in the seamless scroll loop now carry `data-mkt-id` so clicks register regardless of scroll position. Previously only the first copy had the attribute, so once auto-scroll passed the midpoint tapping any index did nothing.  
**Known issues / open work:**
- [ ] Watchlist filter pills not yet built
- [ ] Recommendations section below fold is placeholder only
- [ ] Desktop topmovers: US overnight state needs review
- [ ] Sheet chart does not yet update live as prices tick (price/% update, chart line is static per open)

**How to resume:** Read this file, then open `concepts/mobile/home/index.html`. No need to re-share Figma links — they're above.

## Live URL
GitHub Pages: https://michaeljwilliem.github.io/ajaib/

## Structure

```
ajaib/
└── concepts/
    ├── desktop/
    │   └── topmovers/
    │       └── index.html      # Desktop stock list redesign
    └── mobile/
        └── home/
            └── index.html      # Mobile home screen — canonical edit target + GitHub Pages
```

## Local dev server

Run from `/Users/liem/ajaib`:
```bash
python3 -m http.server 4444
```

- Desktop prototype: http://localhost:4444/concepts/desktop/topmovers/
- Mobile prototype:  http://localhost:4444/concepts/mobile/home/

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

## Mobile Prototype (`mobile/homeconcept/`)

Redesigned mobile home screen — optimized for watchlist density on first fold.

**Design goals:**
- 60–70% of first fold = watchlist
- Multi-asset watchlist (IDX, Crypto, US, Reksa Dana, Obligasi) in one blended list
- Full-width search bar visible (not icon)
- Total aset prominent with full Rp amount (no shortform — clarity is critical for an investment app)

**Layout (top → bottom):**
1. Status bar (iOS, 9:41)
2. App bar: search bar (animated placeholder) + bell + avatar
3. Total Aset: full `Rp X.XXX.XXX` (never abbreviated) + Deposit CTA
4. Row 1 — contextual pills (scrollable): Buying Power (flips IDX/US/Kripto) | Posisi Terbuka | Order Terbuka | E-IPO
5. Row 2 — product shortcuts (scrollable, icon + label): Saham IDX | Kripto | Saham AS | Reksa Dana | Obligasi | Day Trading | Futures | Gold ETF | Lainnya
6. Watchlist (column headers + filter pills + rows)
7. Recommendations (below fold, accessible via scroll)
8. Bottom tab bar (sticky)

**Shortcut row design:**
- Row 1 = "what's happening" — reactive, portfolio-state items; pill format with contextual color (green/amber/blue)
- Row 2 = "where to go" — product discovery; 40px tinted circle icon + 10px label, each asset uses its color token
- Both rows collapse when user scrolls down to watchlist; neither appears in the scroll-mini sticky bar
- Row 2 ordering: most accessible → most specialized (IDX → Kripto → US → RD → OBL → Day Trading → Futures → Gold ETF → Lainnya)

**Watchlist column layout:** Aset | Grafik | Harga | Pergerakan

**Watchlist row flex layout:**
- `winfo` is `flex:1` — the grower; expands to fill all available space so ticker + chips never wrap
- Sparkline is fixed `flex:0 1 44px` — compact visual, not dominant; collapses toward 0 on very narrow screens
- OBL text column (`wspark.obl-text-col`) stays `flex:1` — text truncates with ellipsis
- `wpa` / `us-pa` (price + % area) is `flex-shrink:0` — always protected

**Narrow screen breakpoints:**
- ≤375px (iPhone mini): right-column widths tighten slightly
- ≤340px (Fold outer): further compression, row gap reduces to 6px
- ≤295px (Fold inner): sparkline hidden entirely, logo shrinks to 28px

**Asset type chips + badges:**
- IDX: red chip + ⚡`Nx` thunder badge for leveraged stocks (amber) — both on line 1 of ticker row; `flex-wrap:nowrap` prevents wrapping
- US regular hours: blue chip + `24H` badge on ticker row (reminds users US trades 24/5)
- US extended hours: no badge on ticker row — `24h` chip appears in the price area instead (see below)
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

**US extended hours:** two-line row (62px tall class) — unified `24h` chip label on top line (not per-session "After Market"/"Pre-Market"/"Overnight") + `last close` dimmed below. Top line shows extended session price + `epct`; bottom line shows regular close price + `pct` (that day's regular session change).

**US regular hours:** single-line row (48px) — price + day % only. No two-line layout.

**Live simulation:** IDX, Crypto, US all tick every 1500ms with flash animations. US state switchable via controls outside phone frame.

**UX copy (Indonesian):**
- Edit list: "Ubah Urutan"
- Add to watchlist: "Tambahkan aset ke watchlist kamu"

**iOS phone frame:** 407px outer shell, 387×839px screen, Dynamic Island, side buttons, status bar, home indicator.

## Design principles (from product owner)

- Clarity on amounts is non-negotiable — full Rp numbers, never abbreviated in primary contexts
- Indonesian users don't know US stocks trade 24/5 — always educate contextually
- IDX post-market data is still meaningful — show day change even when market is closed
- Buying power and deposit CTA should always be accessible without scrolling
- Watchlist is the primary feature — maximize its vertical real estate on first fold

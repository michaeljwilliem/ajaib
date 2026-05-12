# Fast Trade — Mobile Prototype

## What this is

A redesigned Fast Trade order book screen for Ajaib's mobile trading app. Built as a static HTML/CSS/JS prototype inside the phone shell used by other Ajaib mobile concepts.

---

## Design brief

Trader users cited Mirae Asset's Fast Order screen as more intuitive than Ajaib's current Fast Trade. The goal was to take what works in Mirae and merge it with Ajaib's design system — not a copy, but a principled synthesis.

**Source designs assessed:**
- Ajaib Fast Trade (Default) — Figma node `6857:66405`, file `l1bqrecCfiJRVrA4nhvDwy`
- Mirae Asset Fast Order screen (user-supplied screenshot)

---

## What we kept from Mirae

| Pattern | Reason |
|---|---|
| 5-column order book: Beli \| Bid \| Harga \| Offer \| Jual | Removes redundant `+` buttons per row; each column is a full tap target |
| Tap a cell to place order instantly | Eliminates the separate buy CTA; action is inline with the data |
| W (Withdraw) buttons in the totals footer | One-tap cancel-all per side, always visible |
| Capacity summary above controls | Traders need to know their limits *before* touching anything |
| 20 levels deep, scrollable | More depth = more context for limit order placement |

## What we dropped from Mirae

- Dated light color scheme
- SUM label (replaced with labeled totals row)
- Non-semantic price coloring

## What we kept from Ajaib

- Dark theme (`#0d0d0f` background)
- Blue = buy / Red = sell color language throughout
- DT / Regular (TL) product toggle
- PRO / LITE switcher in nav
- Badge system on stock ticker

## What we improved over both

- **Spread row removed** — redundant; last price is in nav bar
- **No bottom CTA button** — action is directly on the order book row
- **Segmented controls** match iOS conventions (dark pill container, raised active state) — same pattern for both the Beli/Jual/Fast tabs and the DT/Regular toggle
- **Lot stepper with keyboard input** — tap the number to type directly; falls back to `−/+` for small adjustments
- **"LOT" unit label** below the number in the stepper — eliminates ambiguity about what the number represents (Nielsen: match between system and real world)
- **Capacity strip is dynamic** — Max Limit and Max Lot recalculate when user switches DT ↔ Regular

---

## Layout

```
Screen: 387 × 839px (inside standard phone shell)

Status bar (54px, Dynamic Island)
Nav bar (52px) — back | BBCA ticker + price | ⓘ | LITE/PRO
Segmented control (51px) — Beli | Jual | ⚡ Fast [active]
Order book header (30px) — BELI | BID | HARGA | OFFER | JUAL
Order book scroll (flex: 1, overflow-y: auto)
  └─ 20 ask rows × 32px
  └─ 2px last-price divider
  └─ 20 bid rows × 32px
Totals + W row (40px)
──── sticky bottom panel ────
Capacity strip (36px) — Max Buy | Max Limit (DT/TL) | Max Lot
Controls row (56px) — [DT ⚡20x][Regular TL 6x]  [−][n lot][+]  [MAX]
```

**Default scroll position:** order book auto-scrolls on load so the last-price divider is vertically centered (showing ~8 ask rows above, ~8 bid rows below).

---

## Capacity values

| Product | Max Limit | Max Lot (@ BBCA 8,050) |
|---|---|---|
| DT 20x | Rp400,0M | 496 lot |
| Regular TL 6x | Rp120,0M | 148 lot |

Max Lot = `floor(Max Limit / (price × 100 shares/lot))`

---

## Interactions

| Action | Behaviour |
|---|---|
| Tap Beli cell on any row | Places buy order at that price × current lot count; cell shows lot total; row gets amber outline highlight |
| Tap Jual cell on any row | Same for sell side |
| Tap W (red) | Cancels all pending buy orders; clears cell indicators |
| Tap W (blue) | Cancels all pending sell orders |
| Tap lot number | Opens numeric keyboard; blur validates and clamps to [1, max lot] |
| Tap − / + | Decrements / increments lot by 1, clamped to [1, max lot] |
| Tap MAX | Sets lot to current product's max lot |
| Switch DT ↔ Regular | Recalculates Max Limit + Max Lot; clamps current lot if over new max |

---

## Files

```
concepts/mobile/fast-trade/
├── index.html   — full prototype (single file, no dependencies except Google Fonts)
└── context.md   — this file
```

---

## Open questions / next steps

- [ ] Should Beli / Jual tabs switch the active side (highlighting Beli vs Jual column) or remain purely decorative labels?
- [ ] Confirm multipliers: DT 20x and TL 6x — are these the correct Ajaib product limits?
- [ ] Order confirmation flow — currently a toast; consider a bottom sheet for larger lot sizes
- [ ] Real-time tick simulation for demo purposes
- [ ] Pending order state persistence across price levels (currently in-memory only)

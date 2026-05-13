# USD Prime — Handoff Document

**Project:** USD Taskforce — USD Prime relaunch
**Screen:** Cash Page (L2 Nav) — USD Prime card + detail + deposit flow
**Figma:** https://www.figma.com/design/o66ygPV7ZfBKiol8ai3p1e/Cash-Page-v1?node-id=6977-90954
**Prototype:** `/concepts/mobile/usd-taskforce/index.html`
**Base reference:** `/concepts/mobile/cash/index.html` (Cash v1.0)
**Status:** 🟢 V2.3 — open for iteration

---

## Goal

Relaunch the USD wallet as **USD Prime** — a premium, multi-purpose USD account (save + invest + move) positioned against the BI $25K/month limit on FX purchases. Compete with Wise/Revolut on transparency while maintaining bank-grade gravity for Prime HNWI clients ($100K–$1M+ deposits).

Not a "USD savings account." Not "Rekening Valas." A standalone product name that does the marketing work in the UI.

---

## Strategic Context

BI lowered the monthly USD purchase limit (no underlying assets) from $50K to $25K. BCA / CIMB / Jenius / Jago are capping users. Our USD account is **exempt** because it's structurally classified as for investment purposes (Saham AS, USD Bonds settlement, future USD MF/Bonds/Stablecoins). The relaunch makes this advantage visible without sounding like fine print.

---

## Product Name

**USD Prime** — chosen over `Rekening Valas` (too saving-y), `Investasi USD` (too vertical-locked), and standalone `USD`. Carries tier identity inside the name itself, frees the visual top-right slot for the yield badge, future-proofs upsell ladder.

To change: update `WALLETS.usd.name` plus a few hardcoded strings (`USD Prime` in confirm dir-strip and success caption).

---

## Screen Anatomy

### Main (Cash) screen — USD Prime card
```
┌──────────────────────────────────┐  .wc.tap.prime (188px, navy gradient + gold border)
│ [USD icon]      [Yield 4,75% p.a.]│  top-right yield badge (replaces PRIME ribbon)
│                                  │
│ USD Prime                        │  .wc-name
│ $24,500.00                       │  .wc-bal (tabular)
│ ≈ Rp 399.260.000                 │  .wc-sub-bal (tabular)
│                                  │
│ ↗ +$96.98 bulan ini              │  .wc-yield-strip (green)
│ [$5,000 diproses ›]              │  .bdg-proc (settling, RDN-pattern)
└──────────────────────────────────┘
```

### Detail screen (sub-detail) — single-scroll body
```
┌──────────────────────────────────┐  60px fixed (only sticky-at-rest portion)
│ ‹ USD Prime              ⋮       │  .det-nav
├──────────────────────────────────┤  ↓ everything below scrolls
│ •••• •••• 4821    [Yield 4,75%]  │  .det-hero-top
│ $24,500.00                       │  .det-amount (30px)
│ ≈ Rp 399.260.000                 │  .det-amt-sub
│ ↗ +$96.98 bulan ini              │  .det-yield
│ [⏱ $5,000.00 diproses · AAPL ›]  │  .det-settling-chip → opens settling sheet
│ [Deposit USD] [Tarik]            │  .det-acts
│ [● 1 USD = Rp 16.297 ↑0,3%  ›]   │  .fx-compact → opens FX sheet
│ Diawasi OJK · LPS · Custodian    │  .trust-strip
├──────────────────────────────────┤  ↓ sticky once hero scrolls past
│ Riwayat Transaksi  [30 hari ▾]🔍⬇│  .det-sticky → .txn-statement-hdr
│ [Semua][Masuk][Keluar][Yield]    │  .txn-filter (pill chips)
│ [Konversi][Saham AS]             │
├──────────────────────────────────┤
│ Transactions grouped by date     │  .det-txn-list
└──────────────────────────────────┘
```

Sticky-area budget: ~7% (nav only) at rest, ~22% (nav + filter bar) when scrolled. Below your 30% ceiling.

### Deposit Amount screen (sub-deposit-amount)
```
┌──────────────────────────────────┐
│ ‹ Deposit USD                    │
├──────────────────────────────────┤
│ Jumlah                            │
│ [ Dalam IDR ────────  162,400,000]│  bidirectional input row 1
│ [ Dalam USD ──────────  10,000.00]│  bidirectional input row 2 (synced)
│                                   │
│ [$1,000][$5,000][$10,000][$25,000]│  quick chips
│ [$100,000][Maks $1M]              │
│                                   │
│ Metode Pembayaran                 │
│ ● Tabungan Ajaib · Instan         │  ← all 4 always visible, no dropdown
│ ○ RDN · Instan                    │
│ ○ Virtual Account / Bank Lainnya  │
│ ○ OTC Deposit · Prime · RM        │
│                                   │
│ [RM panel — only when OTC sel'd]  │
│ [Breakdown — hidden when OTC]     │
│ Tanpa batas deposit · POJK        │
├──────────────────────────────────┤
│ [        Lanjutkan        ]       │  CTA → "Hubungi RM" if OTC
└──────────────────────────────────┘
```

### Deposit Confirm screen
```
┌──────────────────────────────────┐
│ ‹ Konfirmasi                     │
├──────────────────────────────────┤
│ Tabungan Ajaib  ›  USD Prime     │  .dir-strip
│ Rp 162.400.000                   │  .conv-ladder
│      ↓                            │
│ × Rp 16.240 / USD                │
│      ↓                            │
│ $10,000.00                        │
│                                   │
│ Rincian Kurs                     │
│ Kurs Tengah BI    Rp 16.297      │
│ Kurs Anda         Rp 16.240      │
│ Spread (Rp 57/USD) Rp 570.000    │
│ Biaya layanan     Rp 0           │
│                                   │
│ Diawasi OJK · LPS · Custodian    │
├──────────────────────────────────┤
│ [⌒60] [   Konfirmasi Deposit   ] │  lock-ring + CTA in footer
└──────────────────────────────────┘
```

### Deposit Success screen
- Animated check ring → check
- Hero: $X masuk ke USD Prime
- Yield projection card (+$X/bulan, proyeksi 1 tahun)
- Meta: ref/tanggal/estimasi aktif
- Utility chips: [Atur Berkala] [Unduh Bukti]
- Primary CTA: Lihat Detail Rekening
- Text link: Selesai

---

## Design Tokens

| CSS var | Value | Usage |
|---|---|---|
| `--bg` | `#06090d` | Main bg |
| `--nav-bg` | `#000d14` | Tab bar bg |
| `--sf` | `#171f2c` | Card surface |
| `--surface-2` | `#1f2a3d` | Elevated surfaces (fx-card, method rows, chips) |
| `--surface-3` | `#243349` | Deepest elevation |
| `--bd` | `#2b3b4e` | Borders |
| `--t1` | `#fff` | Primary text |
| `--t2` | `#849dad` | Secondary text |
| `--t3` | `#5a6e7f` | Tertiary text |
| `--ab` | `#165bcc` | Primary CTA / active nav |
| `--abl` | `#0758db` | Lighter blue (Deposit button) |
| `--db` | `#3a83f9` | Decorative blue (settling, links) |
| `--dbd` | `#02225d` | Processed/info badge bg |
| `--gr` / `--good` | `#2f9d5b` / `#30d158` | Yield/in green |
| `--rd` | `#f65858` | Out/expired red |
| `--prime` | `#c9a96e` | Warm gold — borders + OTC accent |
| `--prime-dim` | `#6b5840` | Dimmed gold |
| `--mid-mkt` | `#5ac8fa` | Mid-market FX rate indicator |

**Font:** Gabarito (400, 500, 700, 900). `.tabular` class applies `font-variant-numeric: tabular-nums` to all numeric displays.

---

## Component Inventory

| Component | Class / ID | Notes |
|---|---|---|
| Premium card | `.wc.tap.prime` | Navy gradient + gold-tinted border, radial highlight via `::before` |
| Yield badge | `.bdg-yield` | Green pill — used identically on USD Prime and Tabungan cards |
| Settling badge | `.bdg-proc` | Blue pill — `$5,000 diproses`, consistent with RDN/Aset Global |
| Yield strip | `.wc-yield-strip` | Green line, `↗ +$X bulan ini` |
| Detail nav | `.det-nav` | 60px fixed top |
| Detail body | `.det-body` | Single scroll container (flex:1, overflow-y:auto) |
| Detail hero | `.det-hero` | Scrolls within body |
| Settling chip | `.det-settling-chip` | Tappable, opens `#sheet-settling` |
| FX compact | `.fx-compact` | 2-line: BI rate + your rate + hemat vs BCA |
| Trust strip | `.trust-strip` | OJK / LPS / Custodian credentials |
| Sticky bar | `.det-sticky` | `position:sticky; top:0` — sticks after hero scrolls past |
| Statement header | `.txn-statement-hdr` | "Riwayat Transaksi" + 30-hari chip + search + download |
| Filter pills | `.txn-ftab` | Pill style (not underline), 6 filters incl. Yield/Konversi/Saham AS |
| Method card | `.dep-method` | Always-visible payment method row with radio |
| RM panel | `.rm-panel` | Shows when OTC selected; avatar + msg + Chat/Telepon |
| Bidirectional input | `.dep-bidi` | Two synced IDR/USD inputs, no swap button |
| Quick chip | `.dep-chip` | $1K → Maks $1M |
| Breakdown | `.dep-breakdown` | Kurs/Spread/Biaya — hidden when OTC selected |
| Direction strip | `.dir-strip` | `From › To` on confirm screen |
| Conversion ladder | `.conv-ladder` | Send ↓ Rate ↓ Receive — Wise-style |
| Lock ring | `.lock-ring` | SVG circle stroke-dashoffset animation, 60s countdown, footer-positioned |
| Success ring | `.success-check-ring` | Animated draw + check pop |
| Yield projection | `.yield-projection` | Green-tinted card with monthly + yearly forecast |
| FX sheet | `#sheet-fx` | Full BI rate + buy/sell + spread + Hemat vs BCA |
| Settling sheet | `#sheet-settling` | Total / Tersedia / Diproses breakdown + source trade |
| Sheet backdrop | `#sheet-backdrop` | Shared dimmer |

---

## Interactions

| Target | Behavior |
|---|---|
| USD Prime card | Opens detail sub-screen |
| `$5,000 diproses` badge | Bubbles to card tap (opens detail) |
| Detail acct settings (⋮) | Toast (placeholder) |
| Settling chip in hero | Opens `#sheet-settling` |
| FX compact row | Opens `#sheet-fx` |
| `Deposit USD` action | Opens `sub-deposit-amount` |
| `Tarik` action | Toast (out of scope) |
| Statement header tools | Toast each (period, search, download) |
| Filter pills | Filter transaction list by type/icon |
| IDR input | Live updates USD input |
| USD input | Live updates IDR input |
| Quick chips | Fills both inputs |
| Method selection | Updates selected state; OTC reveals RM panel + changes CTA |
| RM Chat / Telepon | Toast (placeholder) |
| Lanjutkan (non-OTC) | → confirm screen |
| Lanjutkan (OTC) | Toast "Permintaan terkirim" |
| Lock ring expires (60s) | Disables CTA, shows "Perbarui" link |
| Konfirmasi Deposit | → success screen |
| Success copy ref | Toast "Disalin" |
| Atur Berkala / Unduh Bukti | Toast (placeholders) |
| Lihat Detail Rekening | Back to detail |
| Selesai / X | Back to main cash |

---

## Decisions

| # | Decision |
|---|---|
| Naming | "USD Prime" replaces "Investasi USD" / "Rekening Valas USD". Marketing-positioned. |
| Premium identity | Gradient + gold border + name carries it; PRIME ribbon dropped. |
| Yield placement | Top-right badge (matches Tabungan convention) + monthly earnings line below balance. |
| Settlement display | Uses `.bdg-proc` "diproses" pattern (consistent with RDN/Aset Global). |
| Detail scroll | Single scroll container; only nav (60px) sticky at rest, filter bar becomes sticky on scroll. ≤30% sticky budget. |
| Statement page | Banking-app pattern: title + period chip + search + download icons; filter as pill chips. |
| Payment methods | All 4 visible inline (high-context SEA pattern), no dropdown. Order: Tabungan → RDN → VA/Bank Lainnya → OTC. |
| Bidirectional input | Two synced inputs, no swap button (it's a deposit, not a calculator). |
| OTC flow | RM-assisted: RM panel + Chat/Telepon + CTA flips to "Hubungi RM untuk Deposit". |
| Lock timer | SVG ring + numeric center, positioned in footer adjacent to CTA. |
| Success CTAs | 1 primary + 1 text link + 2 utility chips. Reduced from 4 stacked buttons. |
| Number format | USD uses US convention ($24,500.00), IDR uses Indonesian (Rp 162.400.000). |
| Transaction types | in / out + icon: deposit / withdraw / interest / swap / trade / qris / transfer. Trade rows can have `pending: true` + `pill: 'Settle T+1'`. |

---

## Asset References (Figma MCP — fetched 2026-05-13)

| Asset | URL |
|---|---|
| Header bg (1002×1002) | `https://www.figma.com/api/mcp/asset/76775bf4-4692-4634-a0fc-0e81e52e12f5` |
| Tabungan Ajaib icon | `https://www.figma.com/api/mcp/asset/a3f90e85-2009-4ce9-9fc5-52eab1d4d5ff` |
| RDN icon | `https://www.figma.com/api/mcp/asset/ad49ca12-f8a0-469d-b3b6-6cd83f2398aa` |
| Rekening Aset Global icon | `https://www.figma.com/api/mcp/asset/d8b6c60c-d912-463e-9131-ba1a8c318b5f` |
| USD Wallet icon | `https://www.figma.com/api/mcp/asset/eac32420-d9dc-4be6-a028-979fa8845077` |
| Stockback globe | `https://www.figma.com/api/mcp/asset/79c842f6-6625-44b3-ac02-d78e60a65e5d` |
| Banner 1 | `https://www.figma.com/api/mcp/asset/1c249ab1-4510-423a-bd35-d7c2e4c573f2` |
| Banner 2 | `https://www.figma.com/api/mcp/asset/e99663f1-5def-4555-890d-33b5e3f74e0e` |
| Banner 3 | `https://www.figma.com/api/mcp/asset/55077cb5-ce25-48ed-b012-9ca26c9a52e1` |

---

## Version History

| Ver | Highlights |
|---|---|
| V1 | Cash v1.3 base — Gabarito font, dark navy palette, basic USD wallet card |
| V2.0 | Wise/Revolut-tier upgrade: gradient card, fx-card, limit-card, bidirectional deposit, conversion ladder, lock ring, yield projection |
| V2.1 | Simplification pass: dropped redundant cards from detail hero, compact fx-row, single-CTA success, banking-style statement header |
| V2.2 | US Stock settlement awareness, OTC RM-assisted method, statement page polish, methods all-visible |
| V2.3 | (current) Method order Tabungan→RDN→VA→OTC, single-scroll detail body with sticky filter bar, renamed "Investasi USD" → "USD Prime", PRIME ribbon replaced with yield badge, "settling" → "diproses" |

---

## Open Items

- [ ] Confirm "USD Prime" as final name (vs USD Plus, USD Hub, USD One)
- [ ] Figma pixel comparison against node `6977:90954` for new components
- [ ] Empty state design (when balance = $0 for new users)
- [ ] Tarik USD flow (currently toast)
- [ ] 30-day sparkline on detail hero (deferred from V2.0)
- [ ] Full FX rate history sheet (1D/1W/1M/1Y tabs)
- [ ] Real RM assignment data + recurring deposit flow + PDF receipt

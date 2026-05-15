# Referral Pro — Handoff Document

**Project:** Referral Pro — commission-based referral program for Prime users
**PRD:** https://ajaib.atlassian.net/wiki/spaces/PLAT/pages/2333671440
**Figma:** EkjUZjwS2rjLRwihGKeByi (unavailable at build time — prototype based on PRD)
**Prototype:** `/concepts/mobile/referral-pro/index.html`
**Live:** https://michaeljwilliem.github.io/ajaib/concepts/mobile/referral-pro/
**Status:** 🟢 V1.0 — open for iteration

---

## Goal

Launch a commission-based referral layer ("Referral Pro") for Prime users running in parallel with the existing one-time reward program ("Referral Lite"). Pro earns 15% of brokerage fees from every referee trade — ongoing, uncapped. Targets the IDS product vertical. Entry via Profile → Hadiah Saham, tab switcher Lite | Pro.

---

## Strategic Context

Referral Lite has a ceiling: one reward per friend, incentive stops after KYC. Pro converts active traders into a distribution channel with compounding income potential — closer to an affiliate model. Key conversion insight for SEA: personal identity + recurring earnings beat flat reward, so the Pro entry screen leads with the referrer's own commission earned (not program features).

---

## Screen Anatomy

### Screen 1 — Referral Hub (main / landing)

Tab switcher: **Lite | Pro**

**Lite tab**
```
┌──────────────────────────────────┐
│ Hero card — "hingga Rp100 Juta"  │  .lhero (deep purple gradient)
│ [Bagikan Kode Ajaib]             │  .share-cta (blue CTA, immediate action)
│ Kode: ManagementEventUNJ [Salin] │  .code-card
│                                  │
│ Cara kerjanya (benefit list)     │  .benefit-list
│ • Berdua dapat hadiah saham      │
│ • 100% pasti dapat saham         │
│ • Saham perusahaan top Indonesia │
│                                  │
│ Stats strip (3 rows)             │  .stats-list
│  ▸ 1 Hadiah Saham Tersedia       │
│  ▸ 10 Teman Terdaftar            │
│  ▸ Bonus Tersedia                │
└──────────────────────────────────┘
```

**Pro tab**
```
┌──────────────────────────────────┐
│ [★ Referrer Pro · IDS · 15%]     │  .pro-hero-tag (gold badge)
│ Total komisi: Rp274.500          │  .pro-hero-val — commission as hero
│ dari 8 teman yang sudah trading  │
│ Progress bar: 8/12               │  .pro-progress
│ [8 Sudah Trading] [4 Belum]      │  .pro-pp pills
│                                  │
│ [Bagikan Link Pro]               │  .share-cta (gold variant)
│ Kode: MICH-PRO-2026 [Salin][Share│  .pro-code
│                                  │
│ Keuntungan Referral Pro          │  .pro-blist
│ • Komisi dari setiap transaksi   │
│ • Tanpa batas penghasilan        │
│ • Pantau performa real-time      │
│                                  │
│ ═══ [Lihat Dashboard Referral Pro│  .sticky-cta (gold, position:absolute bottom)
└──────────────────────────────────┘
```

### Screen 2 — Pro Dashboard

```
┌──────────────────────────────────┐
│ ‹ Kembali      Dashboard Pro     │
├──────────────────────────────────┤
│ Total komisi diterima: Rp274.500 │  .dash-hero
│ Progress: 8 dari 12 teman        │
│ [Minggu ini][Bulan ini*][Semua]  │  .period-bar
│                                  │
│ Daftar Referee (12 rows)         │  .ref-list
│  AK  Andi Kurniawan [Trading]    │
│      IDS             +Rp125.000  │
│  CD  Citra Dewi      [KYC]       │
│      Crypto          –  [Ingatkan│
│  EW  Eko Wibowo      [Daftar]    │
│      IDS             –  [Ingatkan│
│  ...                             │
│                                  │
│ Riwayat Komisi                   │  .rwd-list
│  15 Mei 2026                     │
│  ✓ Komisi dari Ivan K.  +Rp28.000│  Dijadwalkan Senin
│  ✓ Komisi dari Kartika  +Rp21.000│
│  12 Mei 2026                     │
│  ✓ Komisi dari Gani H.  +Rp45.000│  Dibayar
└──────────────────────────────────┘
```

### Screen 3 — Referee Welcome

Separate entry point — not reachable from product UI (prototype navigation only via controller sidebar).
```
┌──────────────────────────────────┐
│ ✕ Tutup    Undangan Referral     │
├──────────────────────────────────┤
│       [ML]  avatar ring          │  .ref-avatar-ring (blue glow)
│   Kamu diundang oleh             │
│   Michael Liem                   │
│   untuk bergabung di ajaib       │
│                                  │
│ Hadiah untuk kamu:               │  .ref-reward-card (gold tinted)
│  $ Diskon biaya broker           │
│  ✓ Berlaku tiap transaksi        │
│  🔒 Aman & diawasi OJK           │
│                                  │
│ 3 langkah untuk mulai:           │  .ref-steps
│  1. Daftar akun Ajaib            │
│  2. Selesaikan verifikasi KYC    │
│  3. Lakukan trading pertama ✦    │  (gold — reward trigger step)
│                                  │
│ [Daftar Sekarang]                │
│ Sudah punya akun? Masuk          │
└──────────────────────────────────┘
```

---

## Controller / Navigator

Desktop sidebar (`position:sticky;top:24px`) + mobile notch dropdown (`.di` pill tap). 4 destinations:

| Button | `data-s` | Navigates to |
|---|---|---|
| Lite | `lite` | Hub — Lite tab |
| Pro | `pro` | Hub — Pro tab |
| Dashboard | `dash` | Screen 2 |
| Referee | `ref` | Screen 3 |

Active state syncs automatically on all navigation (tab switch, push/pop, controller tap). JS: `ctrlGo(screen)` → navigate + `syncCtrl(screen)` → update `.on` class.

---

## Design Tokens

Same System B palette as `home/` and `usd-taskforce/`:

| CSS var | Value | Usage |
|---|---|---|
| `--s0` | `#000` | Screen bg |
| `--s1` | `#0d0d10` | Card surface |
| `--s2` | `#1c1c1f` | Elevated surfaces |
| `--s3` | `#242428` | Input / deepest |
| `--b1` | `rgba(255,255,255,.07)` | Subtle borders |
| `--b2` | `rgba(255,255,255,.13)` | Strong borders |
| `--t1` | `#f2f2f7` | Primary text |
| `--t2` | `#8e8e93` | Secondary text |
| `--t3` | `#48484a` | Tertiary / muted |
| `--green` | `#30d158` | Trading / earned |
| `--amber` | `#ffd60a` | KYC / scheduled |
| `--blue` | `#0a84ff` | Primary CTA / Lite accent |
| `--prime` | `#c9a96e` | Gold — Pro identity |
| `--prime-bg` | `rgba(201,169,110,.12)` | Gold tinted fill |
| `--prime-bd` | `rgba(201,169,110,.28)` | Gold border |

**Font:** Inter 400/500/600/700. `.n` class: `font-variant-numeric:tabular-nums`.

---

## Component Inventory

| Component | Class / ID | Notes |
|---|---|---|
| Lite hero | `.lhero` | Deep purple gradient, star decoration |
| Share CTA | `.share-cta` | Blue (Lite) / gold-dark bg (Pro) |
| Referral code card | `.code-card` / `.pro-code` | Salin + Bagikan actions inline |
| Benefit list | `.benefit-list` / `.pro-blist` | Icon + title + sub rows |
| Stats strip | `.stats-list` | Tappable rows with right CTA label |
| Pro hero | `.pro-hero` | Earnings amount + progress bar (gold gradient) |
| Pro tag badge | `.pro-hero-tag` | "Referrer Pro · IDS · 15% Komisi" pill |
| Progress bar | `.pro-progress` / `.dash-progress` | Animated fill on mount (`.6s` ease) |
| Pro stage pills | `.pro-pp` | `trading` (green) / `waiting` (gray) |
| Sticky Dashboard CTA | `.sticky-cta` | `position:absolute;bottom:0` — gradient fade, Pro tab only |
| Period filter | `.period-bar` + `.pbtn` | Minggu ini / Bulan ini / Semua — updates stats + lists |
| Referee row | `.rrow` | Avatar · name · stage pill · product · commission or "Ingatkan" |
| Stage pill | `.spill` | `.r` gray (Daftar) · `.k` amber (KYC) · `.t` green (Trading) |
| Nudge action | `.rnudge` | Blue tap — shows for non-trading referees only |
| Reward row | `.rwrow` | Icon (paid green / sched amber) · desc · amount · status |
| Referee welcome hero | `.ref-welcome-hero` | Navy gradient, avatar ring with glow |
| Reward card | `.ref-reward-card` | Gold-tinted, 3 reward rows |
| Steps | `.ref-steps` | Numbered, step 3 highlighted gold (reward trigger) |
| Share sheet | `#sh-share` | WhatsApp / Salin Link / Telegram / Lainnya |
| Backdrop | `#bd` | Shared dimmer + blur for sheet |
| Toast | `#tst` | 2.2s auto-dismiss |

---

## Interactions

| Target | Behavior |
|---|---|
| Tab Lite / Pro | `tab('lite'|'pro')` — pane fade-in, indicator slides, sticky CTA shows/hides |
| Share CTA (both tabs) | `openShare()` → share sheet |
| Salin (code cards) | `copyCode(id, code)` — label flips to ✓ for 1.8s |
| Sticky "Lihat Dashboard" CTA | `openDash()` → pushes Screen 2 |
| Kembali (dashboard) | `closeDash()` → pops back to Pro tab |
| Period pills (dashboard) | `period(p)` — updates hero val, progress, referee stat, reward list |
| Ingatkan (referee row) | `nudge(name)` → toast "Pengingat dikirim ke [name] via WhatsApp" |
| Controller button | `ctrlGo(screen)` → direct navigation + syncs active state |
| `.di` notch tap (mobile) | `toggleCtrl()` → dropdown panel + pill label turns blue + chevron rotates |
| ✕ Tutup (referee welcome) | `closeRef()` → pops back |

---

## Decisions

| # | Decision |
|---|---|
| Commission-as-hero | Pro tab leads with "Rp274.500" earned, not program description. Goal Gradient: user sees progress, not pitch. |
| Action before information | Share CTA placed immediately after hero on both tabs — user can act without reading benefit list. |
| No cap / recurring framing | "Tiap kali mereka trading, bukan hanya sekali" is the key differentiator vs Lite. Repeated in benefit copy. |
| Sticky Dashboard CTA | `position:absolute;bottom:0` with `pointer-events:none` wrapper, only CTA button re-enables. Gradient from transparent to `rgba(0,0,0,.98)` lets content scroll underneath. |
| Progress bar on Pro | Goal Gradient Effect — 8/12 feels achievable, not overwhelming. Reinforces momentum. |
| Stage-aware referee list | Trading rows show commission; non-trading rows show "Ingatkan" — WhatsApp-first nudge pattern for SEA. |
| Inactive nudge = WhatsApp | `nudge()` fires WhatsApp-first toast. No in-app message — trust the channel users already use. |
| Referee welcome (Screen 3) | OJK trust signal + 3-step clarity + personal referrer identity. Step 3 (trading) highlighted gold — visible reward trigger. Only accessible via controller sidebar in prototype. |
| Controller sidebar vs in-screen link | Removed "Lihat tampilan undangan temanmu" link from Pro screen — Screen 3 is internal/prototype-only, not a user-facing deeplink from the referrer hub. |
| Mobile notch = controller | Follows `home/` pattern. `.di` pill becomes "Navigator" label with chevron on `max-width:640px` / `pointer:coarse`. Drops a 2×2 grid of screen tiles. |

---

## Mock Data

```js
REFS  — 12 referees: 8 trading (t), 2 KYC (k), 2 Daftar (r). Mix of IDS + Crypto products.
RWD   — Reward history keyed by period: week (2 rows) / month (7 rows) / all (same as month).
PS    — Period stats: { val, tr, tot, sub } for week / month / all.
SL    — Stage labels: { r:'Daftar', k:'KYC', t:'Trading' }.
```

To add a referee: push to `REFS` array and add corresponding entries to `RWD` periods.

---

## Version History

| Ver | Highlights |
|---|---|
| V1.0 | Initial build: 3 screens (Hub Lite/Pro, Dashboard, Referee Welcome), share sheet, period filter, nudge, sticky CTA, controller sidebar |

---

## Open Items

- [ ] Confirm 15% commission rate + IDS-only scope vs multi-product (PRD still DRAFT)
- [ ] Referee reward amount (PRD references broker fee sharing but no exact value for referee side)
- [ ] Pro eligibility criteria — how does a user become a Referrer Pro? (not defined in PRD)
- [ ] Empty state for new Pro users (zero commission, zero referees)
- [ ] "Daftar Berkala" / auto-remind scheduled nudges flow
- [ ] Payout schedule detail screen (currently "Dijadwalkan Senin" is a string)
- [ ] Real Figma file review once MCP server is available (EkjUZjwS2rjLRwihGKeByi)

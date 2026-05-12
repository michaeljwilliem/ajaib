# Ajaib Terminal — Desktop Prototype (Alpha)

> Handoff doc. Read this first.

## Status: COMPLETE ✓

All 17 scenes built, financially verified, copy in Bahasa Indonesia. Prototype live. Handover DOCX delivered. Handed off to video production team.

**Live prototype:** https://michaeljwilliem.github.io/ajaib/concepts/desktop/terminal-ai/

**Local dev:**
```
cd /Users/liem/Workspace/ajaib && python3 -m http.server 4444
→ http://localhost:4444/concepts/desktop/terminal-ai/
```

---

## What this is

Single-file HTML prototype (`index.html`) covering every scene needed for the **Ajaib Terminal** board pitch video. **Desktop-first** — the Terminal is the newly released desktop trading app. 17 scenes (S0, S0b, S1–S15); the AI layer inside the app is called **Alpha**.

**Deadline:** Board presentation Friday. Hard video deadline Wednesday EOD. Screen mocks needed Tuesday EOD.

---

## Product framing (read carefully — this is the spine)

- **Ajaib Terminal** = the desktop trading app (newly released). The product.
- **Alpha** = the AI layer that lives inside Terminal. The voice.
- The pun: "Alpha helps you find alpha." Generating alpha (excess return) is the trader's whole job; Alpha (the AI) helps you do it. The name *is* the promise.
- Terminal is where you work; Alpha is who works with you.
- This is an **aspirational vision video** for board purposes. Not all features need to ship today. Vision-state scenes are marked in captions.

## Companion device framing

- Desktop = where you **think** (analysis, deep dive, plan)
- Mobile = where you **act** (push notifications, glanceable digests, on-the-go signals)
- The retail bridge (S13/S14) is the *handoff* between these — same Alpha, same insight, different surface.

---

## Reference files

- `../../../../uploads/Ajaib Terminal Board Presentation.pdf` — original video treatment (8 scenes, 75–80s, no-VO, music-driven, Claude/Higgsfield reference)
- `../../mobile/terminal-signature/index.html` — visual signature exploration → **Aurora gradient won**, **Pulse Dot mark won**
- `../../mobile/terminal/index.html` — the mobile prototype (predecessor; useful component reference for `.t-card`, `.t-mark`, `.t-pill`, etc.)
- Figma references (auth-walled; ask owner for screenshots):
  - Desktop App ID Stocks Phase 1: `https://www.figma.com/design/qBOOXR9BcEQRIyH1xB8QY7/...`
  - Desktop App ID Stocks Phase 2: `https://www.figma.com/design/6sTS09JPY72FH3RfgYk6e4/...`
  - Landing screen node: `6663-58456` (within Phase 1)

---

## Locked design decisions — do not change without asking

| Decision | Locked value | Why |
|---|---|---|
| AI layer name | **Alpha** | Finance double-meaning (alpha = excess return). Bilingual. Pairs cleanly with Terminal. |
| Visual direction | **Aurora** — gradient violet → cyan | Reads "AI" instantly at video speed. |
| Alpha mark | **Pulse Dot** — cyan dot in violet radial halo, breathes via `dotPulse` 1.8s animation | Distinct from Gemini's spark. |
| Accent palette | violet `#8B7CFF` + cyan `#6EE7F7` | Distinct from existing Ajaib red/green/blue. |
| Alpha panel placement | **Option B — third column** (right of Order Book) | Reads as "AI joins your workspace." Both Order Book and Alpha visible. Strong demo visual. |
| Retail bridge | **Companion handoff** (desktop thinking → mobile push) | Continuity, not fragmentation. Same Alpha, different surface. |
| Reference OS | **macOS chrome** (traffic lights, etc.) | Most polished for video reference. |
| Architecture | Single HTML file, scenes as JS template literals, sidebar chip selector | Same pattern as mobile prototype. |
| Language | **Bahasa Indonesia** for all Alpha voice and in-app copy | App is for Indonesian retail traders. |

---

## Design tokens

```css
--tm1: #8B7CFF        /* Alpha primary — violet */
--tm2: #6EE7F7        /* Alpha accent — cyan */
--tm-grad: linear-gradient(135deg, #8B7CFF 0%, #6EE7F7 100%)
--tm-bg:   linear-gradient(135deg, rgba(139,124,255,.20) 0%, rgba(110,231,247,.08) 100%)
--tm-bd:   rgba(139,124,255,.45)
```

Alpha mark (`.t-mark`): 16px circle, violet radial-gradient background, cyan `::after` dot (6px) with `dotPulse` animation. Size variants for banner/pill/rail icon contexts.

Token names kept as `tm-*` (not `alpha-*`) to ease port from the mobile prototype's component library.

---

## Alpha behavior model — the central UX pattern

Alpha lives in four states across every scene:

### State 1 — Closed (default)
- A slim **right rail** (~44px) on the far right edge of the Terminal window.
- Alpha icon at the top with a soft pulse when there's a new signal. Badge shows queued count.
- Inline Alpha moments still appear across panels independently: orderbook rows glow, chart annotations, watchlist t-pills, news tags.

### State 2 — Open (third column)
- Click Alpha icon → panel slides in from right (~340px wide, ~280ms ease-out).
- **Pushes Order Book left**, chart compresses slightly. Both Order Book and Alpha visible — Alpha is a *new column*, not a replacement.
- Header: Alpha mark + label "Alpha" + close X. Footer: "Tanya Alpha apa saja…" input field.
- Shortcut: `⌘.` to toggle.

### Proactive nudge (transitional state)
- When Alpha has something to say while panel is closed: small Aurora toast floats up from the Alpha icon (Gmail-style peek).
- Example: "1 dari 4 event minggu ini berdampak pada portofoliomu. Earnings BBRI — Selasa, 13 Mei." + "Lihat rincian →"

### Desktop OS toast (off-app moments)
- macOS top-right notification, Alpha-branded (Pulse Dot icon).
- Used for S3 (earnings push) and S9 (ANTM entry window). OS-level when user isn't in Terminal.

---

## Layout architecture

```
<body>
  <div class="sidebar">                ← scene selector (left rail)
    <div class="chips">…</div>
  </div>
  <div class="main">
    <div id="stage" class="stage">
      <div class="desktop-block">      ← desktop frame + caption
        <div class="desktop">          ← macOS window (chrome + content)
          <div class="dt-chrome">…</div>
          <div class="dt-screen">      ← Ajaib Terminal app
            <div class="app-nav">…</div>        ← left nav (Watchlists, Screener, …)
            <div class="app-wl">…</div>         ← watchlist column
            <div class="app-chart">…</div>      ← center chart
            <div class="app-ob">…</div>         ← order book (right)
            <div class="app-rail">…</div>       ← Alpha rail (far right, always visible)
            <div class="app-alpha">…</div>      ← Alpha panel (slides in when open)
            <div class="app-acct">…</div>       ← account manager (bottom)
          </div>
        </div>
        <div class="cap">…</div>
      </div>
    </div>
  </div>
</body>
```

Grid CSS:
```css
.app { grid-template-columns: 56px 200px 1fr 240px 44px; }
.app.alpha-open { grid-template-columns: 56px 200px 1fr 240px 340px 44px; }
```

For S13 (companion handoff): desktop at `scale(0.5) opacity(0.4)` left + phone frame right.
For S15 (hero close): phone PIP at `position:absolute; bottom:18px; right:18px` inside `.desktop`.

---

## Scene plan (17 scenes) — all complete ✓

| # | Scene | Alpha state | Key beat |
|---|---|---|---|
| S0 | Chaos hook | — | Browser desktop, 20 tabs, 4 notification popups. Pre-Alpha noise. |
| S0b | Terminal launches | Closed | Clean landing. Alpha pulse in rail. First glimpse. |
| S1 | Earnings calendar | Nudge → open | Feed panel. Alpha nudge: "1 dari 4 event berdampak pada portofoliomu. BBRI — Selasa, 13 Mei." |
| S2 | Fundamental | Open | BBRI detail. Third-column Alpha: consensus, P/E, target, suggested follow-ups. |
| S3 | Earnings toast | Closed | macOS OS toast: "Earnings BBRI 48 jam lagi. Kamu pegang 22% portofolio." |
| S4 | Smart money | Open | BBRI OB. Row 4.805 glows Aurora — Broker MG. Alpha explains accumulation pattern. |
| S5 | Broker flow | Closed (inline) | Broker distribution. MG bar Aurora. Inline t-card — no panel needed. |
| S6 | Accumulation alert | Open | Alpha panel shows order draft: Beli Limit BBRI 4.805 · 113 lot. Vision-state. |
| S7 | Macro strip | Nudge → open | ANTM chart. Gold cell Aurora. Alpha bridges macro → equity: beta 1.28, r=0.82. |
| S8 | Foreign flow | Closed (inline) | ANTM foreign flow. Last 3 bars Aurora. "3,4× velocity" inline tag. |
| S9 | Entry toast | Closed | Full macOS desktop, no app. OS toast: "Peluang entry ANTM ~12 menit lagi." |
| S10 | Chart + Alpha | Open | TLKM chart. 3 cyan annotations: Lonjakan Vol · Waran +540% · Beli Insider. Alpha narrative panel. |
| S11 | Cluster | Open | Alpha panel cluster grid: TLKM 94% · ISAT 81% · EXCL 76%. Pattern match pre-accumulation. |
| S12 | Corp timeline | Closed (inline) | BBRI chart. Timeline strip: 5 past events + 1 future cyan pulse dot (13 Mei). |
| S13 | Companion handoff | Cross-device | Desktop fades. Phone push: "BBRI order limit siap dikirim." Same Alpha, different surface. |
| S14 | Morning brief | Mobile | Phone solo. Pre-market digest: 3 portfolio actions in Indonesian. |
| S15 | Hero close | Open | All Alpha moments active. Phone PIP. "The edge. For everyone." |

---

## Data constants — keep identical across all scenes

| Ticker | Price | Change | Key data |
|---|---|---|---|
| BBRI | 4,820 | +1,26% | Largest holding · 22% · 113 lot · Avg 4.760 · Earnings Selasa 13 Mei · Consensus Buy 28/28 · P/E 9,2× · Target 5.420 · **ARA 6.426 · ARB 3.094** |
| ANTM | 5,940 | +2,42% | 8% portofolio · 1.890 lot · Avg 5.750 · Beta gold 1,28 · Korelasi r=0,82 · **ARA 7.762 · ARB 3.738** |
| TLKM | 3,540 | −0,56% | 42 lot · Avg 3.560 · Vol spike 3,2× · Waran +540% · Insider buy 18,4M @3.450 · **ARA 4.806 · ARB 2.314** |
| MDKA | 2,640 | +1,92% | Beta gold 1,44 · Lagging ANTM today · Catch-up potential +0,7% |

ARA/ARB calculated at ±35% of previous close per IDX auto-rejection rules.

**Broker MG** (Trimegah): accumulating BBRI at 4.805 · 142K lot/hari · 4 sesi berturut-turut. This is the central smart-money signal across S4/S5/S6/S13.

**Macro (S7):** IHSG 7.142 · Gold $3.248 +1,41% · USD/IDR 16.284 · US 10Y 4,48% · Crude $63.8 · Coal $118 · Nickel $16.840

---

## Alpha voice conventions

- **Language:** Bahasa Indonesia · informal "kamu" · English financial terms kept (earnings, consensus, beta, cluster)
- **Register:** Confident, declarative. "BBRI adalah posisi terbesarmu — 22% portofolio." Never hedging.
- **Label format:** `Alpha · [Kategori]` — e.g. "Alpha · Smart Money", "Alpha · Makro", "Alpha · Sinyal 1"
- **Numbers:** Indonesian convention — `9,2×` (comma decimal) · `Rp 5.420` (full stop thousands) · `miliar` not `M`

---

## Component library

Ported from mobile prototype with minor sizing tweaks:

| Class | Component |
|---|---|
| `.t-card` | Aurora insight card |
| `.t-mark` | Pulse Dot sigil |
| `.t-pill` | Inline light-touch callout |
| `.t-cta` | Alpha-tinted text link |
| `.ob-tbl tr.hot` | OB hot row (Aurora highlight) |
| `.macro-strip` / `.macro-cell.hot` | Macro indices strip |
| `.bv-*` | Broker flow view |
| `.ff-*` | Foreign flow chart |
| `.order-draft` / `.od-*` | Order draft UI (S6) |
| `.cluster-grid` / `.cluster-item` | Cluster viz (S11) |
| `.ch-timeline` / `.tl-*` | Corp timeline strip (S12) |
| `.os-toast` | macOS OS notification |
| `.alpha-nudge` | Rail peek bubble |
| `.phone-frame` / `.pf-*` | Phone frame (S13/S14/S15) |
| `.companion-block` | Desktop + phone split (S13) |
| `.hero-pip` | Phone PIP inset (S15) |

---

## File layout

```
/Users/liem/Workspace/ajaib/concepts/desktop/terminal-ai/
├── index.html                              ← THE PROTOTYPE — all 17 scenes
├── Ajaib Terminal Alpha — Video Handover.docx  ← handover doc for video team
└── context.md                             ← this file
```

The mobile prototype lives at `../../mobile/terminal/index.html` — preserved as reference. Do not modify.

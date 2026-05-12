# Ajaib Terminal — Desktop Prototype (Alpha)

> Handoff doc. Read this first.

## What this is

Single-file HTML prototype (`index.html`) covering every scene needed for the **Ajaib Terminal** board pitch video. **Desktop-first** — the Terminal is the newly released desktop trading app. 16 scenes; the AI layer inside the app is called **Alpha**.

**Deadline:** Board presentation Friday. Hard video deadline Wednesday EOD. Screen mocks needed Tuesday EOD.

**Live preview:**
```
cd /Users/liem/Workspace/ajaib && python3 -m http.server 4444
→ http://localhost:4444/concepts/desktop/terminal-ai/
```

---

## Product framing (read carefully — this is the spine)

- **Ajaib Terminal** = the desktop trading app (newly released). The product.
- **Alpha** = the AI layer that lives inside Terminal. The voice.
- The pun: "Alpha helps you find alpha." Generating alpha (excess return) is the trader's whole job; Alpha (the AI) helps you do it. The name *is* the promise.
- Terminal is where you work; Alpha is who works with you.
- This is an **aspirational vision video** for board purposes. Not all features need to ship today. Mark vision-state clearly in scene captions.

## Companion device framing

- Desktop = where you **think** (analysis, deep dive, plan)
- Mobile = where you **act** (push notifications, glanceable digests, on-the-go signals)
- The retail bridge (S13/S14) is the *handoff* between these — same Alpha, same insight, different surface.

---

## Reference files

- `../../../../uploads/Ajaib Terminal Board Presentation.pdf` — original video treatment (8 scenes, 75–80s, no-VO, music-driven, Claude/Higgsfield reference)
- `../../mobile/terminal-signature/index.html` — visual signature exploration → **Aurora gradient won**, **Pulse Dot mark won**
- `../../mobile/terminal/index.html` — the mobile prototype (predecessor; useful component reference for `.t-card`, `.t-mark`, `.t-pill`, etc.). Most components port directly to desktop with minor sizing.
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

---

## Design tokens

```css
--tm1: #8B7CFF        /* Alpha primary — violet */
--tm2: #6EE7F7        /* Alpha accent — cyan */
--tm-grad: linear-gradient(135deg, #8B7CFF 0%, #6EE7F7 100%)
--tm-bg:   linear-gradient(135deg, rgba(139,124,255,.20) 0%, rgba(110,231,247,.08) 100%)
--tm-bd:   rgba(139,124,255,.45)
```

Alpha mark (`.t-mark`): 18px circle, violet radial-gradient background, cyan `::after` dot (7px) with `dotPulse` animation. Size variants for banner/pill/rail icon contexts.

Token names kept as `tm-*` (not `alpha-*`) to ease port from the mobile prototype's component library.

---

## Alpha behavior model — the central UX pattern

Alpha lives in two states across every scene:

### State 1 — Closed (default)
- A slim **right rail** (~44px) on the far right edge of the Terminal window.
- Alpha icon at the top with a soft pulse when there's a new signal.
- Below Alpha: other tools (alerts, settings, etc.) if needed.
- Inline Alpha moments still appear across panels: orderbook rows glow, chart annotations, watchlist pills, news tags — independent of panel state.

### State 2 — Open (third column)
- Click Alpha icon → panel slides in from right (~340px wide, ~280ms ease-out).
- **Pushes Order Book left**, chart compresses slightly. Both Order Book and Alpha visible — Alpha is a *new column*, not a replacement.
- Header: Alpha mark + label "Alpha" + close X.
- Body: conversational by default OR showing current contextual signal.
- Footer: input field for free-form questions.
- Shortcut: `⌘.` to toggle.

### Proactive nudge (transitional state)
- When Alpha has something to say while panel is closed: a small Aurora toast floats up from the Alpha icon (Gmail-style peek).
- "Alpha spotted something on ANTM" + click to expand panel with that context loaded.

### Desktop OS toast (lockscreen-equivalent moments)
- macOS top-right notification, Alpha-branded.
- Used for S3 (earnings push) and S9 (BPJS entry window).
- Visually distinct from in-app proactive nudges — these are OS-level when user isn't in Terminal.

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
            <div class="app-sidebar">…</div>   ← left nav (Watchlists, Screener, …)
            <div class="app-wl">…</div>        ← watchlist column
            <div class="app-chart">…</div>     ← center chart
            <div class="app-ob">…</div>        ← order book (right)
            <div class="app-rail">…</div>      ← Alpha rail (far right, always visible)
            <div class="app-alpha">…</div>     ← Alpha panel (slides in when open)
            <div class="app-acct">…</div>      ← account manager (bottom)
          </div>
        </div>
        <div class="cap">…</div>
      </div>
    </div>
  </div>
</body>
```

For S13 (companion handoff), the canvas widens to fit both desktop frame + phone frame side-by-side.
For S15 (hero close), a single wide desktop frame with a phone inset (PIP-style) at bottom-right.

---

## Scene plan (16 scenes)

Source-of-truth list. Status updates as scenes are built.

| # | Scene | Desktop form | Alpha state | Key beat | Status |
|---|---|---|---|---|---|
| S0 | Chaos hook | Browser desktop with 30+ tabs, Slack/WA pings, news widgets | — | Pre-Alpha noise | ⏳ |
| S0b | Terminal launches | Clean landing screen, Alpha icon pulses softly in right rail | Closed, icon pulsing | First glimpse of Alpha | ✓ reference built |
| S1 | Earnings calendar | Feed panel scrolled to calendar | Proactive nudge → panel opens | Alpha picks the event that matters | ⏳ |
| S2 | Fundamental | Stock detail (chart + orderbook), Alpha panel open with conversational explainer | Open | "Why BBRI matters to you" | ✓ reference built |
| S3 | Desktop toast | macOS top-right toast: "Alpha · BBRI earnings in 48h" | Closed | Pre-app proactivity | ⏳ |
| S4 | Smart money | Stock detail with orderbook hot row + Alpha panel breakdown | Open | Multi-panel anomaly highlight | ⏳ |
| S5 | Broker flow | Broker distribution view, inline Alpha t-card | Closed (inline-only) | Light-touch — no panel needed | ⏳ |
| S6 | Accumulation alert | Alpha panel takes focus with order draft | Open | Alpha proposes an action | ⏳ |
| S7 | Macro strip | Top market strip lights Gold cell | Proactive nudge → panel opens | Macro → equity bridge | ⏳ |
| S8 | Foreign flow | Foreign flow chart with Alpha acceleration indicator inline | Closed (inline) | Real-time anomaly | ⏳ |
| S9 | Desktop toast | Time-sensitive macOS toast | Closed | Urgent proactivity | ⏳ |
| S10 | Chart + Alpha | Chart with cyan annotations across vol/warrant/director filing | Open | Stacked observations | ⏳ |
| S11 | Cluster | Alpha panel full view: 3 stocks similar pre-accumulation pattern | Open | Deepest pattern read | ⏳ |
| S12 | Corp timeline | Stock detail tab, Alpha timeline annotation | Closed (inline) | Historical anchoring | ⏳ |
| **S13** | **Companion handoff** | Desktop fades → person away from desk → phone push from Alpha | — (cross-device) | Bridge moment | ⏳ |
| **S14** | **Mobile morning brief** | Phone solo, Alpha pre-market digest with 3 portfolio actions | — (mobile) | Mobile companion at strength | ⏳ |
| S15 | Hero close | Wide desktop frame with Alpha across panels + phone PIP showing same Alpha on mobile | Open | "Ajaib Terminal · Powered by Alpha · The edge. For everyone." | ⏳ |

---

## Component library (pulled forward from mobile prototype)

These port directly with minor sizing tweaks. Don't reinvent.

| Class | Component | Purpose |
|---|---|---|
| `.t-card` | Aurora insight card (primary surface) | Inline t-cards inside panels, OR contents of the open Alpha panel |
| `.t-mark` | Pulse Dot sigil | Inside every Alpha surface (panel header, t-card head, pill, etc.) |
| `.t-pill` | Inline pill (light-touch callout) | Watchlist rows, indices strip, news headlines |
| `.t-cta` | Alpha-tinted text link | Inside t-cards |
| `.hl-row.hot` | In-content highlight row | Orderbook hot rows, calendar day highlights |

## New desktop-only components (to design)

| Class | Component |
|---|---|
| `.desktop` / `.dt-chrome` / `.dt-screen` | macOS window frame |
| `.app-sidebar` | Left vertical nav (Watchlists, Screener, Portfolio, Market, Feed, Profile) |
| `.app-wl` | Watchlist column with stock list |
| `.app-chart` | Center chart panel (candlestick + timeframe controls + indicators) |
| `.app-ob` | Order Book panel (right) |
| `.app-acct` | Account Manager (bottom; Aset/Order/Riwayat + Jual/Beli) |
| `.app-rail` | **Alpha rail** — slim right edge, always visible, icon + pulse dot |
| `.app-alpha` | **Alpha panel** — third column when open |
| `.alpha-nudge` | Proactive toast peeking from Alpha icon |
| `.os-toast` | macOS top-right notification (Alpha-branded) |
| `.companion` | Desktop + phone split layout for S13 |
| `.hero-frame` | S15 wide desktop + phone PIP composition |

---

## Conventions

- **Data consistency across scenes:** BBRI = largest holding (22%, 113 lots, avg Rp4.760, price 4,820 +1.26%, earnings Tue 13 May). ANTM = gold-correlated (beta 1.28, 8%, 1,890 +2.42%). TLKM = unusual volume (3.2× avg, warrant +540%). Broker MG accumulating BBRI at 4,805 (4 sessions, 142K lots/day). **Keep these identical across all scenes.**
- **Voice:** Alpha speaks in confident, declarative sentences. "BBRI earnings in 48 hours. You hold 22%." Not "There might be earnings…"
- **Label format:** `Alpha · [category]` (e.g. "Alpha · Earnings", "Alpha · Smart Money", "Alpha · Macro").
- **Language mix:** Bahasa Indonesia for app chrome (Beranda, Saham, Jual/Beli, "Order bisa dikirim"). English for Alpha voice and copy aimed at the board.
- **Tickers used:** BBRI, BBCA, BMRI, ANTM, MDKA, TLKM, ASII, GOTO, INDF, UNVR, ISAT, EXCL, FREN, PTBA. Don't add new ones unless needed.
- **Captions under desktop frames (`sceneCap`):** Always include scene number, timing window, label, what it does, and Alpha state (closed/open/nudge/toast).

---

## What carries over from the mobile prototype

- **All design tokens** (Aurora colors, gradients, mark animation)
- **Component classes** (`.t-card`, `.t-mark`, `.t-pill`, `.t-cta`, `.hl-row.hot`) — port directly
- **Conversational pattern** (`.dd-msg`, `.dd-ref`, `.dd-sug`) — port directly into the Alpha panel
- **Helper functions** (`goTo()`, `sceneCap()`) — same pattern
- **Architecture** (single HTML, scenes as template literals, sidebar chips)
- **Data narrative** (BBRI/ANTM/TLKM stories, numbers, MG broker accumulation)

## What's net-new for desktop

- macOS window chrome (traffic lights)
- Multi-panel layout (sidebar + watchlist + chart + orderbook + account manager + rail + Alpha)
- Alpha rail (slim icon strip, far right)
- Alpha panel (sliding third column)
- Proactive nudge (peek bubble from Alpha icon)
- Desktop OS toast (macOS top-right)
- Companion handoff layout (S13, desktop + phone)
- Hero PIP composition (S15)
- S0 chaos as desktop browser tabs (not mobile lockscreen)

---

## Open items / what's next

- **Scenes built:** S0b (landing, Alpha closed), S2 (stock detail, Alpha open). These prove the third-column pattern works visually.
- **Next priority order** for building remaining scenes: S0 → S1 → S4 → S6 → S15 (these have the most narrative weight in the video).
- **S13 companion handoff** needs a creative call — is it a *cinematic cut* (desktop scene fades, person+phone shown separately) or a *split-frame composition* (desktop + phone in the same shot)? Default plan: split-frame for clarity, but a cut might be more emotional.
- **S15 hero PIP** — wide desktop frame with all Alpha moments lit simultaneously + small phone inset bottom-right. Composition needs review.
- **Video copy** ("The edge. For everyone." closer, Alpha tagline, etc.) still unreviewed by Laith/Anderson.
- **macOS chrome accuracy** — current version uses simplified traffic lights + title bar. Could be polished further if board cares about fidelity.

---

## File layout

```
/Users/liem/Workspace/ajaib/concepts/desktop/terminal-ai/
├── index.html       ← THE PROTOTYPE — all scenes here
└── context.md       ← this file
```

The mobile prototype lives at `../../mobile/terminal/index.html` and is preserved as reference / historical handoff. Don't modify it unless we revisit the mobile-only path.

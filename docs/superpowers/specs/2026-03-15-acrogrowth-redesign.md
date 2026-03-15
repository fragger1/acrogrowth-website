# AcroGrowth Landing Page — Full Visual Redesign Spec
**Date:** 2026-03-15
**Approach:** Full Atomic Rebuild (Approach 1) — discard all existing CSS, rewrite from scratch

---

## Business Context

- **Product:** AcroGrowth — B2B outbound lead generation and appointment-setting agency
- **Core offer:** 8 qualified meetings/month for B2B Tech companies
- **Channels:** LinkedIn outbound (Sales Navigator, InMail, connection sequences) + LinkedIn content creation
- **Target audience:** B2B Tech founders and CEOs with inconsistent pipelines
- **Risk reversal:** Full refund if 8 meetings not delivered
- **Entry point:** Free 2-week pilot
- **Calendly embed:** https://calendly.com/leomanhal/new-meeting
- **Future addition:** VSL (YouTube video) under hero — placeholder required now

---

## Design Direction

**Style:** Elevated Brutalist Grid
- Newspaper-meets-agency aesthetic
- Asymmetric editorial columns where appropriate
- Dense typographic hierarchy
- Black structural borders and rules as the primary design element
- Zero border-radius on all elements
- No gradients, no shadows, no soft colors

---

## Design System

### Typography

| Role | Font | Weight | Notes |
|------|------|--------|-------|
| Display / Headlines | `Barlow Condensed` | 800 | Ultra-condensed, architectural, precise |
| Body / UI | `Instrument Sans` | 400, 500, 600 | Clean, neutral |
| Section labels | `Instrument Sans` | 600 | `0.2em` tracking, uppercase, 11px |

Google Fonts: `Barlow+Condensed:wght@800&family=Instrument+Sans:wght@400;500;600`

Section label color inherits from the section's text color: black on white sections, white on black sections.

### Color Tokens

```css
--black: #000000;
--white: #FFFFFF;
--border-dark: 2px solid #000000;   /* white sections */
--border-light: 2px solid #FFFFFF;  /* black sections */
```

No grays. No gradients. No shadows. Contrast via background flips and borders only.

### Spacing & Grid

- Max content width: `1140px`, `margin: 0 auto`
- Section padding: `96px 40px` desktop → `64px 24px` at ≤900px
- All `border-radius: 0` everywhere, no exceptions

### Breakpoints

- `≤900px` — tablet/mobile: most multi-column layouts collapse to single column
- `≤600px` — small mobile: font size scaling, tighter padding

### Section Alternation Rhythm

| Section | Background | Text Color | Border Var |
|---------|-----------|------------|------------|
| Nav | White | Black | `--border-dark` |
| Hero | White | Black | `--border-dark` |
| VSL Placeholder | White | Black | `--border-dark` |
| Problem | **Black** | White | `--border-light` |
| How It Works | White | Black | `--border-dark` |
| Offer | **Black** | White | `--border-light` |
| FAQ | White | Black | `--border-dark` |
| Book a Call | **Black** | White | `--border-light` |
| Footer | White | Black | `--border-dark` |

### Animations

- **Page load (hero):** Staggered `fadeUp` — eyebrow (0.1s delay) → headline (0.25s) → horizontal rule (0.35s) → two-column row (0.5s). Each: `opacity 0→1`, `translateY 24px→0`, `duration 0.7s ease`. Applied via `.fade-in-1` through `.fade-in-4` classes.
- **Scroll reveal:** `IntersectionObserver` (threshold `0.12`) triggers `.visible` on `.reveal` elements. Elements that get `.reveal`: section labels, section headlines, cards, checklist items, step cells, FAQ items, book-a-call content blocks. Animation: `opacity 0→1`, `translateY 30px→0`, `duration 0.6s ease`, `transition`. Hero elements use the load animation instead, not scroll reveal.
- **Hover states:** All buttons, nav cells, problem cards: `background-color` and `color` invert in `transition: 0.15s ease`.
- **Ticker:** CSS-only `@keyframes ticker` — `translateX(0) → translateX(-50%)`, `20s linear infinite`. DOM must contain two copies of ticker content for seamless loop.
- **FAQ accordion:** `max-height: 0 → 300px` with `overflow: hidden`, `transition: 0.35s ease`. Icon: `rotate(0deg) → rotate(45deg)` and `background-color: transparent → black, color: white`.

### External Dependencies

- Google Fonts: `https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@800&family=Instrument+Sans:wght@400;500;600&display=swap`
- Font Awesome: `https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css`
- Calendly CSS: `https://assets.calendly.com/assets/external/widget.css`
- Calendly JS: `https://assets.calendly.com/assets/external/widget.js` (async, before `</body>`)

---

## Section Specifications

### 1. NAV

- `position: fixed`, `top: 0`, `left: 0`, `right: 0`, `z-index: 100`
- `height: 60px`, white bg, `border-bottom: var(--border-dark)`
- `display: flex`, `align-items: center`, `justify-content: space-between`
- Padding: `0 32px` desktop → `0 20px` at ≤900px

**Logo lockup (left):**
- `display: flex`, `align-items: center`, `gap: 10px`
- `favicon.svg` at `height: 28px; width: auto`
- "ACROGROWTH" in `Barlow Condensed` 800, `font-size: 24px`, `letter-spacing: 0.04em`, `color: black`, `text-decoration: none`

**Segmented toolbar (right):**
- `display: flex`, `align-items: stretch`, `height: 38px`
- Outer wrapper: `border: 2px solid black` on all four sides — wraps all cells as one unit
- Internal cell separators: `border-right: 2px solid black` on each cell except the last
- No gaps between cells — flush, zero-gap flex row
- All cells: `Instrument Sans` 600, `11px`, `letter-spacing: 0.13em`, `text-transform: uppercase`, `display: flex; align-items: center; white-space: nowrap`
- Nav link cells `[Process] [Offer] [FAQ]`: `padding: 0 18px`, white bg, black text. Hover: `background-color: black; color: white` (0.15s ease)
- CTA cell `[Book a Call →]`: `padding: 0 22px`, black bg, white text, no extra border (outer wrapper handles it). Hover: `background-color: white; color: black` (0.15s ease). `href="#book"`

**Mobile (≤900px):**
- Nav link cells `[Process] [Offer] [FAQ]` hidden (`display: none`)
- Only logo lockup + CTA cell remain

---

### 2. HERO

- `min-height: 100vh`, white bg, `border-bottom: var(--border-dark)`
- Padding: `140px 40px 80px` desktop → `120px 24px 80px` at ≤900px
- `display: flex; flex-direction: column; justify-content: center; position: relative`
- Inner wrapper: `max-width: 1140px; margin: 0 auto; width: 100%`

**Structure (top to bottom, all inside inner wrapper):**

**① Eyebrow** — `.fade-in .fade-in-1`
- `Instrument Sans` 600, `11px`, `letter-spacing: 0.2em`, `text-transform: uppercase`, `color: black`
- `display: flex; align-items: center; gap: 12px; margin-bottom: 28px`
- `::before` pseudo: `content: ''`, `display: inline-block`, `width: 32px`, `height: 2px`, `background: black`
- Copy: `B2B OUTBOUND FOR Tech FOUNDERS`

**② Headline** — `.fade-in .fade-in-2`
- `Barlow Condensed` 800, `font-size: clamp(80px, 11vw, 160px)`, `line-height: 0.92`, `text-transform: uppercase`, `letter-spacing: 0.01em`, `color: black`
- `margin-bottom: 36px`, `max-width: 100%`
- Copy (two lines): `8 QUALIFIED MEETINGS.` / `EVERY MONTH.`

**③ Horizontal rule** — `.fade-in .fade-in-3`
- `border: none; border-top: 2px solid black; margin-bottom: 36px`

**④ Two-column row** — `.fade-in .fade-in-4`
- `display: grid; grid-template-columns: 60fr 40fr; gap: 48px; align-items: start`
- A `2px solid black` vertical divider is achieved via `border-left: 2px solid black` on the right column with `padding-left: 48px`

*Left column (60%):*
- Subheadline: `Instrument Sans` 400, `18px`, `line-height: 1.65`, `color: #111`, `max-width: 520px`, `margin-bottom: 36px`
- Copy: *"We run your entire LinkedIn outreach and content machine — so your calendar fills with decision-makers, not noise. Done for you. Guaranteed."*
- CTA button: `background: black; color: white; border: 2px solid black; padding: 16px 40px; font-family: Instrument Sans; font-size: 13px; font-weight: 600; letter-spacing: 0.1em; text-transform: uppercase; text-decoration: none; cursor: pointer; display: inline-block`. Hover inverts. `href="#book"`. `margin-bottom: 20px`
- Guarantee micro-line: `display: flex; align-items: center; gap: 8px; font-size: 13px; font-weight: 500; letter-spacing: 0.04em; color: black`. Icon: `fa-shield-halved` at 14px. Copy: *"Full refund if we miss the target"*

*Right column (40%), with left border divider:*
- `display: flex; flex-direction: column; gap: 12px; padding-left: 48px; border-left: 2px solid black`
- 3 proof-point tags, each: `border: 2px solid black; padding: 8px 16px; font-family: Instrument Sans; font-size: 12px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.12em; color: black; width: fit-content`
- Tag copy: `LinkedIn Outbound`, `Sales Navigator`, `Done For You`

**⑤ Ticker strip** — `position: absolute; bottom: 0; left: 0; right: 0`
- `border-top: 2px solid black; padding: 14px 40px; overflow: hidden`
- Inner track: `display: flex; gap: 80px; white-space: nowrap; animation: ticker 20s linear infinite`
- Items (duplicated twice for seamless loop): `LinkedIn Outbound` · `Sales Navigator` · `InMail Sequences` · `Content Creation` · `ICP Lead Lists` · `Inbound Pipeline` · `8 Meetings / Month` · `Done For You`
- Item style: `Instrument Sans` 600, `12px`, `letter-spacing: 0.14em`, `text-transform: uppercase`. Separator `::after`: `content: '—'; opacity: 0.35; margin-left: 16px`

**Mobile (≤900px):**
- Two-column row collapses to single column (`grid-template-columns: 1fr`)
- Right column's left border removed; displayed below left column with `padding-left: 0; border-left: none; margin-top: 32px`

**Mobile (≤600px):**
- Headline: `font-size: clamp(52px, 14vw, 80px)`

---

### 3. VSL PLACEHOLDER

- White bg, `border-bottom: var(--border-dark)`
- Padding: `48px 40px`
- Inner container: `max-width: 1140px; margin: 0 auto`
- Bordered box: `border: 2px solid black; width: 100%; height: 360px; display: flex; flex-direction: column; align-items: center; justify-content: center; gap: 24px`
- Play icon box: `width: 52px; height: 52px; border: 2px solid black; display: flex; align-items: center; justify-content: center`; icon: `fa-play` at `18px`
- Label: `Barlow Condensed` 800, `24px`, `text-transform: uppercase`, `letter-spacing: 0.04em`, `color: black`. Copy: `OVERVIEW VIDEO — COMING SOON`
- Subtext: `Instrument Sans` 400, `14px`, `color: #111`. Copy: `See exactly how AcroGrowth fills your calendar`
- Designed for swap: replace inner content with `<iframe width="100%" height="100%" src="https://www.youtube.com/embed/VIDEO_ID" frameborder="0" allowfullscreen></iframe>`

**Mobile (≤600px):** Box height: `200px`

---

### 4. PROBLEM

- Black bg, white text, `border-bottom: var(--border-light)`
- 2-column layout: `display: grid; grid-template-columns: 1fr 1fr; gap: 80px; align-items: start`
- Section padding: `96px 40px`

**Left column (sticky):**
- `position: sticky; top: 80px`
- Section label + headline + intro paragraph
- Section label: `——— THE PROBLEM` (white rule via `::before`)
- Headline: `Barlow Condensed` 800, `clamp(44px, 5.5vw, 72px)`, white, `max-width: 480px`, `margin-bottom: 24px`
- Copy: `YOUR PIPELINE SHOULDN'T BE A GUESSING GAME.`
- Intro paragraph: `Instrument Sans` 400, `17px`, `line-height: 1.7`, `opacity: 0.75`, `max-width: 420px`, `margin-top: 8px`
- Copy: *"Most tech founders are doing everything right — great product, solid team — and still staring at an empty pipeline. Here's why."*

**Right column (scrolling cards):**
- 3 cards, `display: flex; flex-direction: column; gap: 0`
- Each card: `border-top: 2px solid white; padding: 36px 32px; position: relative; cursor: default`
- Cards 2 & 3: `border-top: none` (avoids double borders — single border between cards)
- Ghost number: `Barlow Condensed` 800, `56px`, `rgba(255,255,255,0.1)`, `line-height: 1`, `margin-bottom: 12px`
- Card title: `Barlow Condensed` 800, `26px`, white, `margin-bottom: 10px`, `line-height: 1.1`
- Card body: `Instrument Sans` 400, `15px`, `rgba(255,255,255,0.72)`, `line-height: 1.65`
- Hover (desktop): `background-color: white; color: black; transition: 0.2s ease`. Ghost number, title, body all inherit the flip.
- Touch devices: hover state not applied. Cards are static on touch.

**Card content:**

*Card 01 — "Outreach Dies in the Inbox"*
Generic cold messages get ignored. Without a targeted ICP list and proven sequence, you're burning time to generate zero pipeline.

*Card 02 — "Inconsistent Pipeline Every Quarter"*
You close deals, go heads-down on delivery, look up three months later to a dead pipeline. The feast-or-famine cycle isn't a growth strategy.

*Card 03 — "No Time to Do It Yourself"*
LinkedIn content, connection sequences, follow-ups, InMail campaigns — each one is a part-time job. Together, impossible to manage while running a company.

**Mobile (≤900px):**
- Columns stack (`grid-template-columns: 1fr`)
- Left column: `position: static` (not sticky)
- Gap between stacked columns: `48px`

---

### 5. HOW IT WORKS

- White bg, `border-bottom: var(--border-dark)`
- Section label + headline above grid
- Headline copy: `THREE STEPS. ONE CALENDAR FULL.`

**3-column step grid:**
- `display: grid; grid-template-columns: repeat(3, 1fr); gap: 0`
- Outer wrapper: `border: 2px solid black` on all sides
- Each step cell: `padding: 48px 36px; position: relative; border-right: 2px solid black`
- Last cell: `border-right: none`

*Per cell:*
- Ghost step number: `position: absolute; top: 16px; right: 20px; font-family: Barlow Condensed; font-weight: 800; font-size: 96px; line-height: 1; color: rgba(0,0,0,0.06); pointer-events: none; user-select: none`
- Icon square: `width: 48px; height: 48px; border: 2px solid black; display: flex; align-items: center; justify-content: center; font-size: 20px; margin-bottom: 28px`
- Step title: `Barlow Condensed` 800, `28px`, `text-transform: uppercase`, `letter-spacing: 0.02em`, `margin-bottom: 14px`, `line-height: 1.1`
- Step body: `Instrument Sans` 400, `15px`, `line-height: 1.65`, `color: #111`

**Step content:**

*Step 01 — icon `fa-crosshairs` — "WE BUILD YOUR ICP LEAD LIST"*
We use Sales Navigator to identify your exact buyers — filtered by title, company size, industry, and buying signals — then build a verified, high-intent target list.

*Step 02 — icon `fa-linkedin-in` (FA brands) — "WE RUN OUTREACH + CREATE CONTENT"*
We execute multi-touch LinkedIn sequences — connection requests, InMails, follow-ups — while publishing content on your profile that attracts inbound leads.

*Step 03 — icon `fa-calendar-check` — "YOU GET MEETINGS ON YOUR CALENDAR"*
Qualified prospects book directly. You show up, close deals. 8 qualified meetings per month, guaranteed.

**Mobile (≤900px):**
- Grid becomes `grid-template-columns: 1fr`
- Step cells: `border-right: none; border-bottom: 2px solid black`
- Last step cell: `border-bottom: none`

---

### 6. OFFER

- Black bg, white text, `border-bottom: var(--border-light)`
- Section label + headline: `EVERYTHING YOU NEED. NOTHING YOU DON'T.`

**Two-panel grid:**
- `display: grid; grid-template-columns: 1fr 1fr; gap: 0; border: 2px solid white`

*Left panel — "What's Included":*
- `padding: 52px 48px; border-right: 2px solid white`
- Panel title: `Barlow Condensed` 800, `28px`, uppercase, `margin-bottom: 32px`
- Copy: `WHAT'S INCLUDED`
- 5-item checklist, `display: flex; flex-direction: column; gap: 20px`:
  - Each item: `display: flex; align-items: flex-start; gap: 14px`
  - Icon box: `width: 24px; height: 24px; border: 2px solid white; display: flex; align-items: center; justify-content: center; flex-shrink: 0; margin-top: 2px`. Icon: `fa-check` at `11px`, white.
  - Item text block: strong title (`Instrument Sans` 600, `16px`, white, `display: block`) + description (`Instrument Sans` 400, `14px`, `rgba(255,255,255,0.72)`, `display: block; margin-top: 2px`)

  Items:
  1. **8 Qualified Meetings / Month** — Decision-makers who match your ICP, pre-qualified before they hit your calendar.
  2. **Full LinkedIn Outbound Campaign** — Sales Navigator list building, connection sequences, InMail campaigns, and follow-ups — executed for you.
  3. **LinkedIn Content Creation** — We write and post authority content on your profile to drive inbound interest and warm up cold outreach.
  4. **ICP Lead List, Built Fresh Each Month** — Verified, high-intent leads sourced via Sales Navigator — no recycled databases.
  5. **Weekly Reporting** — You see exactly what's running, what's working, and what's booked — no black boxes.

*Right panel — Pilot + Guarantee:*
- `padding: 52px 48px; background: rgba(255,255,255,0.03)`
- "FREE 2-WEEK PILOT" badge: `display: inline-block; background: white; color: black; font-family: Instrument Sans; font-size: 11px; font-weight: 700; letter-spacing: 0.12em; text-transform: uppercase; padding: 6px 14px; margin-bottom: 20px`
- Panel title: `Barlow Condensed` 800, `28px`, uppercase. Copy: `TRY IT BEFORE YOU COMMIT`
- Paragraph: `Instrument Sans` 400, `15px`, `rgba(255,255,255,0.82)`, `line-height: 1.7; margin-bottom: 28px`. Copy: *"Start with a free two-week pilot — no payment required. We'll launch a live campaign, prove the process works for your market, and hand you results you can verify before any contract discussion."*
- CTA button (white outlined): `display: block; width: 100%; text-align: center; background: transparent; color: white; border: 2px solid white; padding: 16px 40px; font-family: Instrument Sans; font-size: 13px; font-weight: 600; letter-spacing: 0.1em; text-transform: uppercase; text-decoration: none; cursor: pointer`. Hover: `background: white; color: black`. `href="#book"`. Copy: `BOOK A CALL TO START`
- Guarantee block: `margin-top: 36px; border-top: 2px solid rgba(255,255,255,0.2); padding-top: 32px`
  - Label: `display: flex; align-items: center; gap: 10px; font-family: Barlow Condensed; font-size: 22px; font-weight: 800; text-transform: uppercase; letter-spacing: 0.04em; color: white`. Icon: `fa-shield-halved` at `16px`, white. Copy: `THE GUARANTEE`
  - Paragraph: `Instrument Sans` 400, `14px`, `rgba(255,255,255,0.8)`, `line-height: 1.65; margin-top: 10px`. Copy: *"If we don't deliver 8 qualified meetings in your first full month, you get a full refund. No fine print, no partial credits. We hold ourselves accountable to the number."*

**Mobile (≤900px):**
- `grid-template-columns: 1fr`
- Left panel: `border-right: none; border-bottom: 2px solid white` (becomes top separator for right panel)
- Right panel: `padding-top: 48px`

**Mobile (≤600px):**
- Panels: `padding: 36px 28px`

---

### 7. FAQ

- White bg, `border-bottom: var(--border-dark)`
- Section label + headline: `FAIR QUESTIONS. STRAIGHT ANSWERS.`
- FAQ list container: `max-width: 760px; margin-top: 0`
- No border on the container itself — item borders provide all framing

**Per accordion item:**
- `.faq-item`: `border-top: 2px solid black`
- Last `.faq-item`: also `border-bottom: 2px solid black`
- Question row (button): `width: 100%; background: none; border: none; display: flex; align-items: center; justify-content: space-between; padding: 28px 0; cursor: pointer; text-align: left; gap: 16px`
- Question text: `Barlow Condensed` 800, `22px`, `line-height: 1.3`, `color: black`
- Icon box (closed): `width: 32px; height: 32px; border: 2px solid black; display: flex; align-items: center; justify-content: center; flex-shrink: 0; font-size: 14px; transition: transform 0.25s ease, background-color 0.2s ease`. Icon: `fa-plus`, black.
- Icon box (open): `background-color: black; color: white; transform: rotate(45deg)`
- Answer: `max-height: 0; overflow: hidden; transition: max-height 0.35s ease`
- Open state: `max-height: 300px`
- Answer inner: `padding-bottom: 28px; font-family: Instrument Sans; font-size: 16px; line-height: 1.7; max-width: 640px; color: #111`
- JS: clicking any item closes all others and opens the clicked one (accordion, not multi-expand)

**5 Questions:**

*Q1: What counts as a "qualified" meeting?*
A qualified meeting means a booked call with a decision-maker — typically a founder, C-suite executive, or VP — at a company that matches your ICP criteria: industry, headcount, revenue range, and any other filters we agree on upfront. If a meeting doesn't meet those criteria, it doesn't count toward your 8.

*Q2: What happens if you don't hit 8 meetings?*
You get a full refund. No partial credits, no rollovers, no "we'll make it up next month." If we miss the number in your first full month of engagement, we return your money. The guarantee exists because we're confident in the system — and because accountability is the only way to build trust.

*Q3: Do I have to do anything once we start?*
Very little. We run the entire outreach operation — list building, content, sequences, follow-ups. You'll spend roughly 30–60 minutes onboarding: sharing your ICP definition, approving messaging, and giving us access to your LinkedIn. After that, your only job is to show up to the meetings we book.

*Q4: How does the free 2-week pilot work?*
We kick off a live campaign at no cost — same process, same tools, same effort as a paying client. In two weeks, you'll see real outreach activity, real responses, and real data. If it's working and you want to continue, we move to a paid engagement. If not, you walk away with zero financial risk.

*Q5: What does LinkedIn content creation actually involve?*
We write and schedule posts directly on your LinkedIn profile — typically 3–5 posts per week — built around your expertise, your ICP's pain points, and topics that attract inbound interest. Strategic content that positions you as a credible resource for the exact buyers you're targeting, so outreach lands warmer.

---

### 8. BOOK A CALL

- Black bg, white text, no bottom border
- 2-column layout: `display: grid; grid-template-columns: 1fr 2fr; gap: 80px; align-items: start`
- Section padding: `96px 40px 120px`

**Left column (sticky context panel):**
- `position: sticky; top: 80px`
- Section label: `——— BOOK A CALL` (white rule)
- Headline: `Barlow Condensed` 800, `clamp(44px, 5vw, 72px)`, white, `margin-bottom: 20px`. Copy: `SEE IF YOU QUALIFY.`
- Subline: `Instrument Sans` 400, `17px`, `rgba(255,255,255,0.85)`, `line-height: 1.65; margin-bottom: 36px`. Copy: *"15 minutes. We'll look at your ICP, your current pipeline, and tell you honestly whether we can deliver for your business."*
- CTA button (white outlined): same spec as Offer panel button above. Copy: `PICK A TIME BELOW`. On desktop this is decorative/contextual — the Calendly widget is visible alongside it. On mobile, clicking scrolls to `#calendly-widget`.
- 3 meta items: `display: flex; flex-direction: column; gap: 16px; margin-top: 32px`
  - Each: `display: flex; align-items: center; gap: 10px; font-family: Instrument Sans; font-size: 14px; font-weight: 500; color: rgba(255,255,255,0.85)`. Icon: white, `13px`.
  - Item 1: `fa-clock` + "15-minute intro call"
  - Item 2: `fa-video` + "Video or phone"
  - Item 3: `fa-shield-halved` + "No commitment required"

**Right column (Calendly):**
- `id="calendly-widget"`
- Wrapper: `border: 2px solid white; overflow: hidden; min-height: 700px`
- Inside: `.calendly-inline-widget` div with `data-url="https://calendly.com/leomanhal/new-meeting"` and `style="min-width:320px;height:700px;"`
- JS init: call `Calendly.initInlineWidgets()` after script loads. Polling fallback: `setInterval` every 300ms until `window.Calendly` exists, then call and `clearInterval`.

**Mobile (≤900px):**
- `grid-template-columns: 1fr`
- Left column: `position: static`
- Right column below left

---

### 9. FOOTER

- White bg, `border-top: 2px solid black`
- `padding: 32px 40px`, `display: flex; align-items: center; justify-content: space-between; flex-wrap: wrap; gap: 16px`

- **Left:** "ACROGROWTH" — `Barlow Condensed` 800, `22px`, `letter-spacing: 0.04em`, black
- **Center:** "© 2026 AcroGrowth. All rights reserved." — `Instrument Sans` 400, `13px`, `color: #444`
- **Right:** nav links — `display: flex; gap: 24px`; each: `Instrument Sans` 500, `13px`, `color: black; text-decoration: none`. Hover: `text-decoration: underline`. Links: `Process → #how-it-works`, `Offer → #offer`, `FAQ → #faq`, `Book a Call → #book`

**Mobile (≤900px):**
- `flex-direction: column; align-items: flex-start; gap: 20px`
- Stacking order: Logo → Nav links → Copyright
- `padding: 28px 24px`

---

## Technical Constraints

- Single `index.html` file — all CSS and JS embedded in `<style>` and `<script>` tags
- No build tools, no npm, no frameworks
- External resources (loaded via CDN): Google Fonts, Font Awesome, Calendly CSS + JS
- `scroll-behavior: smooth` on `html` element. JS `scrollIntoView({ behavior: 'smooth' })` fallback for all `<a href="#...">` clicks
- All primary CTA buttons (`href="#book"`) scroll to the `#book` section
- Mobile-first CSS: base styles for mobile, overrides at `min-width: 901px` and `min-width: 601px`
- Favicon: `<link rel="icon" type="image/svg+xml" href="favicon.svg" />`
- Semantic HTML: `<nav>`, `<section id="...">`, `<footer>`, `<button>` for FAQ toggles
- `overflow-x: hidden` on `body`

---

## Out of Scope

- Testimonials section (not included per original spec)
- Real VSL video (placeholder only — YouTube iframe swap later)
- Any color beyond `#000000` and `#FFFFFF`
- Any backend, form handling, or CMS integration
- CSS frameworks or UI libraries

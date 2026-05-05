# Handoff: Draftflow Marketing SPA

## Overview

This package contains the high-fidelity HTML design reference for the Draftflow marketing website — a single-page, static site served via GitHub Pages from `/docs`. The site targets developers who use Claude Code and explains what Draftflow is, how the `/df` bridge workflow works, how to install it, and answers common questions.

The goal is to implement this as a production-ready static HTML/CSS/JS site (no framework required — vanilla is fine) that can be dropped into `/docs/index.html` of the main repo and deployed via GitHub Pages.

---

## About the Design Files

`Draftflow.html` is a **high-fidelity design reference** built in plain HTML/CSS/JS. It is **not** production code to copy verbatim. Use it as the visual and behavioural specification. The task is to:

1. Recreate the design pixel-faithfully in clean, maintainable HTML/CSS
2. Refactor the feature grid to load from `features.json` (already done in the existing `/docs/index.html` — keep that pattern)
3. Ensure it works cleanly as a GitHub Pages static file with no build step

---

## Fidelity

**High-fidelity.** All colours, typography, spacing, hover states, animations, and copy are final. Implement pixel-precisely.

---

## Design Tokens

```css
--paper:        #f7f4ef;   /* warm off-white — page background */
--paper-dark:   #ede9e1;   /* slightly darker paper — hover bg, note boxes */
--paper-mid:    #f2eee7;   /* mid paper — card hover state */
--ink:          #1a1714;   /* near-black — primary text, primary button bg */
--ink-muted:    #6b6560;   /* secondary text, nav links */
--ink-faint:    #a09890;   /* hints, labels, meta text */
--accent:       #2d5a3d;   /* forest green — accent colour */
--accent-light: #e8f0ea;   /* green tint — badge bg, code bg */
--accent-mid:   #4a8a60;   /* mid green — badge dot */
--border:       rgba(26,23,20,0.10);
--border-strong:rgba(26,23,20,0.20);
```

### Typography

| Role | Font | Weight | Size |
|------|------|--------|------|
| Headlines | Lora (serif) | 500 | clamp(2.4rem, 5.5vw, 3.5rem) hero; 1.75rem sections |
| Body | DM Sans | 300–500 | 1rem base |
| Code / labels / mono | JetBrains Mono | 400–500 | 0.7–0.82rem |

Load from Google Fonts:
```
https://fonts.googleapis.com/css2?family=Lora:ital,wght@0,400;0,500;1,400&family=JetBrains+Mono:wght@400;500&family=DM+Sans:wght@300;400;500;600
```

### Spacing & Layout

- Max content width: `880px`, centred, `padding: 0 2rem`
- Section vertical padding: `5rem 0`
- Border radius: `10px` (cards, terminals, notes)
- Nav height: `56px`

---

## Sections / Views

### 1. Nav

- **Position:** `sticky top:0`, `z-index: 100`
- **Background:** `rgba(247,244,239,0.88)` with `backdrop-filter: blur(14px)`
- **Border-bottom:** `1px solid var(--border)`
- **Height:** `56px`
- **Layout:** flex row, space-between, gap `1.5rem`
- **Logo:** "Draftflow" in Lora 500 1.1rem, followed by a `2px × 1em` green blinking cursor (`animation: blink 1.1s step-end infinite; 50% opacity:0`)
- **Nav links:** `Features`, `Bridge`, `Install`, `FAQ` — DM Sans 0.85rem, `--ink-muted`, hover → `--ink`; hidden on mobile ≤740px
- **GitHub button:** JetBrains Mono 0.78rem, border `1px solid var(--border-strong)`, padding `5px 14px`, border-radius `6px`; hover → bg `--ink`, color `--paper`

---

### 2. Hero

- **Padding:** `6rem 0 0`
- **Badge:** pill shape, `--accent-light` bg, `--accent` text, JetBrains Mono 0.7rem, `border-radius: 100px`; contains a `5×5px` dot (`--accent-mid`) + text "Open source · macOS · MIT"
- **H1:** Lora 500, `clamp(2.4rem, 5.5vw, 3.5rem)`, line-height `1.12`, letter-spacing `-0.025em`, max-width `660px`. `<em>` = italic `--accent`
  - Copy: `A markdown editor built for <em>Claude Code</em> workflows.`
- **Subheading:** DM Sans 300, 1.05rem, `--ink-muted`, max-width `500px`, margin-bottom `2.4rem`
  - Copy: `Write prompts, skills, and drafts in a focused editor. Skill autocomplete, live preview, token counting, and a direct bridge back to Claude Code.`
- **CTA buttons (flex row, gap 1rem):**
  1. **Brew button** (primary green): `--accent-light` bg, `--accent` text, JetBrains Mono 0.8rem, padding `10px 20px`, border-radius `7px`, border `1px solid rgba(45,90,61,0.25)`. Hover → bg `--accent`, color `#fff`, translateY(-1px). Contains house SVG icon + text `brew install --cask draftflow`
  2. **GitHub button** (secondary dark): `--ink` bg, `--paper` text, JetBrains Mono 0.8rem, padding `10px 20px`, border-radius `7px`. Hover → `#2d2925`, translateY(-1px). Contains GitHub SVG icon + `GitHub ↗`
- **Hero note:** JetBrains Mono 0.75rem, `--ink-faint`. Copy: `macOS · Node.js 18+`
- **Scroll reveal:** all hero elements use `opacity:0 → 1` + `translateY(18px → 0)` on IntersectionObserver, staggered 0.08s apart

---

### 3. App Mockup (hero bottom)

A rendered illustration of the Draftflow app window — dark Electron UI. Sits below hero text, no bottom padding on hero (mockup bleeds into rule).

**Window chrome:**
- bg `#1e1c18`, border-radius `12px 12px 0 0`, no bottom border
- Title bar: `#2a2724`, 3 traffic-light dots (red `#e06c75`, yellow `#e5c07b`, green `#98c379`), centred title in JetBrains Mono 0.68rem `rgba(255,255,255,0.25)`

**3-column body:** `grid-template-columns: 220px 1fr 1fr`, min-height `340px`

**Sidebar (220px):**
- bg `#252320`, border-right `rgba(255,255,255,0.05)`
- Two sections: "Project" (3 file items) and "Skills" (3 skill items)
- Section labels: JetBrains Mono 0.6rem, `rgba(255,255,255,0.2)`, uppercase
- Items: flex row, `gap:7px`, JetBrains Mono 0.68rem; active item has `rgba(255,255,255,0.07)` bg
- Colour dots: blue `#61afef` for `.md` files, yellow `#e5c07b` for skills

**Editor pane:**
- bg `#1e1c18`, padding `18px 20px`
- Toolbar: Edit / Split / Preview pills, active has `rgba(255,255,255,0.08)` bg
- Editor content: JetBrains Mono 0.7rem, line-height `1.9`; heading in `#e5c07b`, text `rgba(255,255,255,0.55)`, skill tag `#98c379` with `rgba(152,195,121,0.1)` bg
- Blinking cursor: `1.5px × 0.9em`, `#98c379`
- **Autocomplete popup** (position relative, flows in DOM between editor content and suggestions strip):
  - bg `#2a2724`, border `rgba(255,255,255,0.1)`, border-radius `7px`, width `210px`
  - Header: "Skills — type # to search" in 0.58rem uppercase
  - 3 items; active item has `rgba(152,195,121,0.12)` bg, `#98c379` text
- **Suggestions strip** (contextual, no trigger needed):
  - Flex row, border-top `rgba(255,255,255,0.05)`, padding `8px 0 6px`
  - Label "SUGGESTED" in 0.58rem mono, `rgba(255,255,255,0.2)`
  - 3 chips: pill shape, `rgba(255,255,255,0.05)` bg, border `rgba(255,255,255,0.09)`, JetBrains Mono 0.62rem
  - Each chip: green dot `5×5px` + skill name + muted reason + `×` dismiss button
  - Chips animate in on scroll: staggered `opacity 0→1` + `translateY(6px→0)`, starting 900ms after mockup enters viewport, 120ms apart
  - `×` click dismisses with `opacity:0 + scale(0.85)` transition, then removes from DOM
- **Editor footer:** flex row, border-top `rgba(255,255,255,0.05)`, stats in JetBrains Mono 0.6rem `rgba(255,255,255,0.22)` + "Send back ↩" pill in `--accent`
- **Token counter animation:** counts 0→247 over 1200ms with cubic ease-out when mockup enters viewport

**Preview pane:**
- bg `#262320`, padding `18px 20px`
- Serif rendered markdown preview
- Hidden on mobile ≤740px (sidebar too)

---

### 4. Features Grid

- **Section label:** "Features" — JetBrains Mono 0.68rem uppercase `--ink-faint`
- **Section title:** "Everything you need.\nNothing you don't." — Lora 1.75rem
- **Section sub:** 0.9rem `--ink-muted`, max-width `480px`
- **Grid:** `repeat(3, 1fr)`, `gap: 1px`, border `1px solid var(--border)`, border-radius `10px`, overflow hidden, bg `var(--border)` (gap colour)
- **7 cards total.** The 7th card spans `grid-column: 1 / -1` (full width) since it's alone in row 3.
- **Card:** bg `--paper`, padding `1.5rem 1.6rem`, hover → `--paper-mid`
- **Icon tag:** JetBrains Mono 0.7rem, `--accent` text, `--accent-light` bg, border `rgba(45,90,61,0.15)`, padding `3px 9px`, border-radius `4px`
- **Card title:** DM Sans 500, 0.9rem
- **Card body:** 0.82rem `--ink-muted`; inline `<code>` uses `--accent-light` bg, `--accent` text

**7 cards:**
| Tag | Title | Description |
|-----|-------|-------------|
| `.md` | Markdown editor | Distraction-free writing with live split-pane preview. Edit / split / preview modes. Mermaid diagram rendering. |
| `#skill` | Skill autocomplete | Type `#` to fuzzy-search skills, `/` for agents. Hover to preview a skill without leaving the editor. |
| `/df` | Claude Code bridge | Open files directly from Claude Code, edit them, and send the result back with one click. |
| `∿` | Token counter | Real-time token count powered by `@anthropic-ai/tokenizer`. Know your context before you send. |
| `~/` | Project tree | Browse all files rooted at the nearest `CLAUDE.md`. Recent files panel. Quick-open palette via `⌘P`. |
| `◻` | Scratchpad | Persistent scratch space separate from your draft. Auto-saved to `~/.claude/draftflow-scratch.md`. |
| `✦` | Suggested skills | As you type, relevant skills surface as chips below the editor — no trigger needed. Powered by `claude-haiku-4-5`. Silently off without an API key. |

---

### 5. Bridge / How It Works

- **Section label:** "How it works"
- **Section title:** "The round-trip workflow."
- **4 numbered steps** in a vertical flow with connector lines:
  - Step number: `32×32px` circle, border `1.5px solid var(--border-strong)`, JetBrains Mono 0.68rem `--ink-faint`. Step 01 is "active": border `--accent`, color `--accent`, bg `--accent-light`
  - Connector: `1.5px` wide, bg `var(--border)`, `min-height: 24px`, grows to fill space
  - Content: h4 DM Sans 500 0.9rem + p 0.82rem `--ink-muted`; inline `<code>` uses `--paper-dark` bg, `--accent` text

**Steps:**
1. **Type `/df` in Claude Code** — The slash command writes your content to `~/.claude/editor-bridge/request.md` and opens it in Draftflow via the `draftflow://` URL scheme.
2. **Edit with full autocomplete** — Type `#` for skills, `/` for agents. Contextual suggestions appear as chips as you type.
3. **Click "Send back"** — Draftflow writes to `~/.claude/editor-bridge/response.md`. Claude Code picks it up immediately.
4. **Or use standalone** — Open any `.md` file, write a prompt, and use "Send to Claude" to copy to clipboard.

**Aside box** (below steps):
- bg `--paper-dark`, border `1px solid var(--border)`, border-radius `10px`, padding `1.4rem 1.6rem`
- Label: "Bonus: `/df p`" — JetBrains Mono 0.65rem uppercase `--ink-faint`
- Body: `Opens Claude's previous response — a plan, a summary — in the preview pane for review. Write notes in the editor, then send only the notes back. The plan is never re-sent to Claude.`

---

### 6. Install

- **Section label:** "Quick start"
- **Section title:** "Up and running in seconds."
- **Two terminal blocks** side by side (`grid-template-columns: 1fr 1fr`, gap `1.2rem`, collapses to 1 column ≤640px):
  - Left (Recommended): Homebrew install
  - Right (From source): git clone + npm
- **Third terminal block** below (full width): post-launch `/df` command setup
- **Terminal style:**
  - bg `#1c1a17`, border-radius `10px`
  - Title bar: `#252320`, traffic light dots, label right-aligned JetBrains Mono 0.62rem `rgba(255,255,255,0.2)`
  - Body: padding `1.2rem 1.4rem`, JetBrains Mono 0.78rem, line-height `2`
  - Prompt `$`: `rgba(255,255,255,0.2)`, non-selectable
  - Commands: `#abb2bf`
  - Comments: `rgba(255,255,255,0.18)`
  - Success: `#98c379`
  - Dim text: `rgba(255,255,255,0.28)`
- **Install note box:** bg `--paper-dark`, border `1px solid var(--border)`, border-radius `7px`, padding `1rem 1.2rem`, 0.82rem `--ink-muted`. Copy: `First launch on macOS: the system may block the app. Go to System Settings → Privacy & Security → "Open Anyway", or run xattr -cr /Applications/Draftflow.app.`

**Terminal content — Homebrew:**
```
# Install via Homebrew
$ brew install --cask \
  sameera207/draftflow/draftflow
✓ Draftflow installed
```

**Terminal content — From source:**
```
$ git clone https://github.com/
  sameera207/draftflow.git
$ cd draftflow && npm install
$ npm start
```

**Terminal content — After launch:**
```
# Install the /df Claude Code command (one-time)
  Draftflow → Settings → Install /df command
# Then in any Claude Code session:
$ /df Draft your prompt here…
```

---

### 7. FAQ (Accordion)

- **Section label:** "FAQ"
- **Section title:** "Common questions."
- **6 items** in a borderless accordion
- Each item: border-top `1px solid var(--border)`; last item also has border-bottom
- **Question button:** full-width, flex row, space-between, DM Sans 500 0.88rem; hover → `--accent`
- **Arrow:** `▾` character, 0.68rem `--ink-faint`; rotates `180deg` when open
- **Answer:** 0.85rem `--ink-muted`, line-height `1.75`; `max-height: 0 → 400px` on open, transition `0.32s ease`

**Q&A pairs:**
1. **Do I need a Claude API key?** → No. Draftflow is local. No API key required for core features. Optional key enables contextual skill suggestions (claude-haiku-4-5); silently disabled if absent.
2. **What is skill autocomplete?** → Type `#` for skills, `/` for agents. Hover to preview. Default scan path `~/.claude`.
3. **What are contextual suggestions?** → Proactive skill chips below the editor as you type. Requires Anthropic API key, uses claude-haiku-4-5. Cached per session. Silently disabled without key.
4. **Does it work on Windows or Linux?** → macOS primary. Built on Electron so may work elsewhere, but unsupported.
5. **How does "Send to Claude" work?** → Copies markdown to clipboard. Paste into Claude Code terminal. No browser integration.
6. **Is this open source?** → Yes, MIT license. `github.com/sameera207/draftflow`.

---

### 8. Footer

- Border-top `1px solid var(--border)`, padding `2.5rem 0`
- Flex row, space-between, wraps on mobile
- Left: "Draftflow" in Lora 500 0.9rem `--ink-muted`
- Centre: links — GitHub `https://github.com/sameera207/draftflow`, draftflow.dev `https://draftflow.dev` — 0.78rem `--ink-faint`
- Right: "MIT License" in JetBrains Mono 0.72rem `--ink-faint`

---

## Interactions & Behaviour

### Scroll Reveal
All section headings and content blocks use IntersectionObserver:
```js
{ threshold: 0.08 }
// On intersect: opacity 0→1, translateY(18px→0), transition 0.55s ease
// Stagger delays: 0.08s, 0.16s, 0.24s, 0.32s for siblings
```

### Token Counter Animation
On mockup enter viewport: counts `0 → 247` over `1200ms`, cubic ease-out (`1 - (1-t)^3`).

### Suggestion Chip Entrance
900ms after mockup enters viewport, chips fade in one by one (`120ms` stagger):
```js
opacity: 0 → 1, translateY(6px → 0), transition 0.3s ease
```

### Chip Dismiss
Clicking `×` on a suggestion chip:
```js
opacity: 0, scale(0.85), transition 0.2s ease → remove from DOM after 220ms
```

### FAQ Accordion
One item open at a time. Click open item to close. `max-height` transition for smooth expand/collapse.

### Smooth Scroll
All `href="#..."` anchor links scroll smoothly with `72px` offset for the sticky nav.

---

## Responsive Breakpoints

| Breakpoint | Changes |
|---|---|
| ≤740px | Mockup sidebar + preview pane hidden; nav links hidden |
| ≤680px | Features grid → 2 columns |
| ≤640px | Install options → 1 column |
| ≤600px | Bridge steps → 1 column |
| ≤420px | Features grid → 1 column |

---

## Assets

- **`draftflow-icon.svg`** — App icon SVG (1024×1024 viewBox 128×128). Use as `<link rel="icon">` and for any social OG image.
- **GitHub SVG icon** — Inline in HTML (standard GitHub mark path)
- **Google Fonts** — Lora, DM Sans, JetBrains Mono (loaded via `<link>`)

---

## GitHub Repo

- Repo: `https://github.com/sameera207/draftflow`
- Deploy target: `/docs/index.html` on `main` branch → GitHub Pages → `draftflow.dev`
- CNAME file `/docs/CNAME` must contain: `draftflow.dev`

---

## Files in This Package

| File | Purpose |
|------|---------|
| `Draftflow.html` | High-fidelity design reference — the full SPA |
| `draftflow-icon.svg` | App icon SVG, Concept A, Parchment colour scheme |
| `README.md` | This document |

---

## Implementation Notes for Claude Code

1. **Start from the existing `/docs/index.html`** in the repo — it already has the correct structure and feature card JSON loading. The `Draftflow.html` in this package is the enhanced target state.
2. **Keep the `fetch('features.json')` pattern** for the feature grid — do not hardcode the cards.
3. **No build step needed** — pure HTML/CSS/JS. No bundler, no framework.
4. **The app mockup is an SVG/HTML illustration** — not a screenshot. Recreate it faithfully with the dark colour scheme as shown.
5. **Test at all breakpoints** listed above before shipping.
6. **Add Open Graph meta tags** to the `<head>`:
   ```html
   <meta property="og:title" content="Draftflow — Markdown editor for Claude Code workflows">
   <meta property="og:description" content="Write prompts, skills, and drafts in a focused editor. Skill autocomplete, live preview, token counting, and a direct bridge back to Claude Code.">
   <meta property="og:url" content="https://draftflow.dev">
   <meta property="og:image" content="https://draftflow.dev/og-image.png">
   <meta name="twitter:card" content="summary_large_image">
   ```

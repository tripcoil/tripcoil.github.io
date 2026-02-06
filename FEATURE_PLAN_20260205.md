# Feature Plan: TripCoil Relay Tools Redesign

**Date:** 2026-02-05
**Author:** Feature Intelligence Architect
**Status:** Awaiting Approval

---

## Executive Summary

The TripCoil relay tools are functionally powerful but visually chaotic. Fourteen calculators, each designed independently, create a fragmented experience that exhausts users before they get value. The opportunity: unify these tools under a single, calm design system that matches the new homepage aesthetic. Apply Steve Jobs' principle of removal—hide complexity, reveal power progressively, make the first 10 seconds effortless.

---

## Current State

### What's Working
- **Functionality is solid** — calculations are accurate, formulas display correctly
- **Step-by-step solutions** — differentiator that competitors don't offer
- **Offline-capable** — all client-side, loads fast
- **Mobile responsive** — grids collapse appropriately

### What's Almost There
- **Dark theme exists** — but it's busy (#0f172a with blue accents, borders everywhere)
- **Consistent font stack** — system fonts used, but overuse of monospace
- **Real-time updates** — some tools auto-calculate, others require button clicks

### What's Missing
- **Unified design language** — homepage is black/minimal, tools are dark-blue/busy
- **Visual calm** — too many borders, cards, sections, labels fighting for attention
- **Progressive disclosure** — everything visible at once overwhelms users
- **Delight moments** — purely functional, no emotional connection
- **Navigation between tools** — back link only, no awareness of siblings

### What's at Risk
- **User abandonment** — overwhelming UI makes users leave before getting value
- **Perceived quality** — cluttered design undermines trust in calculations
- **Mobile experience** — dense layouts become unusable on phones

---

## Design Principles (Jobs + Rams)

1. **Subtract until it breaks** — remove every element that isn't essential
2. **Progressive revelation** — show inputs first, reveal complexity on demand
3. **One focal point** — the result is the hero, not the controls
4. **Consistent restraint** — same black background, same type scale, same spacing
5. **Silent confidence** — no "SECTION TITLES", no borders, no visual shouting

---

## Phase 1: Ship This Week

### 1.1 Unified Design Token System

**What it does:** Create a single CSS file (`tokens.css`) that all tools import. Black background (#000), white text, gray secondary (#86868b), consistent spacing scale (8, 16, 24, 48, 80px).

**Why it matters now:** Every subsequent change builds on this foundation. Without it, we're painting different walls different colors.

**What it builds on:** The homepage already uses this palette. Copy it exactly.

**What it doesn't touch:** No JavaScript changes. No functional changes.

**Implementation context:**
- Create `/random/tokens.css`
- Define CSS custom properties: `--bg`, `--text`, `--text-secondary`, `--spacing-*`
- Font stack: `-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif`
- No monospace except for actual code/numbers

---

### 1.2 Remove Visual Noise (All Tools)

**What it does:** Strip out elements that add no value:
- Remove blue underline from h1 headings
- Remove card borders (rely on spacing alone)
- Remove "← Back to Tools" text link (replace with subtle arrow in header)
- Remove uppercase section titles
- Remove redundant labels where units are obvious

**Why it matters now:** Every removed element makes the remaining elements more powerful.

**What it builds on:** Design tokens from 1.1

**What it doesn't touch:** All functionality remains. Just CSS removals.

**Implementation context:**
- Kill `.card { border: 1px solid... }`
- Kill `text-transform: uppercase` on section titles
- Kill `border-bottom` on h1
- Background: pure black, not dark blue (#0f172a → #000)

---

### 1.3 Input-First Layout

**What it does:** On every calculator, the input field(s) appear first, centered, with generous whitespace. Results appear below only after calculation. Controls are minimal.

**Why it matters now:** Users want to *do* something. Show them where to act first.

**What it builds on:** Tokens + noise removal

**What it doesn't touch:** Calculation logic, formulas, step-by-step content

**Implementation context:**
- Container max-width: 680px (matches homepage)
- Input fields: large (48px height), centered
- Remove sidebar navigation (Power Systems Solver has one)
- Single-column layout for mobile-first simplicity

---

### 1.4 Subtle Navigation Header

**What it does:** Replace "← Back to Tools" link with a consistent header across all tools:
- Left: "← TripCoil" (subtle gray link back to homepage)
- Center: Tool name (not a heading, just the name)
- Right: nothing (or minimal action if needed)

**Why it matters now:** Creates cohesion across all 13 tools.

**What it builds on:** Homepage header pattern

**What it doesn't touch:** Tool functionality

**Implementation context:**
- Simple flex header, 60px height
- No borders, no shadows
- Sticky position

---

### 1.5 Auto-Calculate Everything

**What it does:** Remove "Calculate" buttons. Every tool calculates in real-time as inputs change (with debounce for performance).

**Why it matters now:** Every button click is friction. Remove it.

**What it builds on:** Existing `oninput` handlers in some tools

**What it doesn't touch:** Calculation accuracy

**Implementation context:**
- Add `input` event listeners with 100ms debounce
- Remove all "Calculate" buttons
- Remove button styling from CSS (not needed)

---

## Phase 2: Ship This Sprint

### 2.1 Result Hero Pattern

**What it does:** The primary result (fault current, k0 value, sequence components) becomes the visual hero:
- Large typography (32-48px for primary number)
- Subtle unit/label below
- Secondary results in smaller type below that

**Why it matters now:** Users came for an answer. Make it unmissable.

**What it builds on:** Phase 1 layout simplification

**What it doesn't touch:** Step-by-step details (those stay collapsible)

**Implementation context:**
- Result display: centered, large
- Use tabular-nums font feature for number alignment
- Animate result changes subtly (opacity transition)

---

### 2.2 Collapsible Advanced Inputs

**What it does:** For tools with many inputs (Fault Current, Mho Visualizer), show only essential inputs by default. "Show more options" reveals advanced controls.

**Why it matters now:** Reduces overwhelm. 80% of users use 20% of options.

**What it builds on:** Input-first layout from 1.3

**What it doesn't touch:** All options remain accessible

**Implementation context:**
- Group inputs: Essential vs Advanced
- Collapsed state with `+ More options` trigger
- Smooth height animation on expand
- Remember preference in localStorage

---

### 2.3 Unified Phasor/Diagram Styling

**What it does:** Canvas-based visualizations (phasor diagrams, Mho circles, synchroscope) get consistent styling:
- Black background (matches page)
- White/gray grid lines (subtle)
- Consistent color palette for phases (red, green, blue)
- No borders on canvas

**Why it matters now:** Diagrams are a selling point. Make them beautiful.

**What it builds on:** Design tokens

**What it doesn't touch:** Calculation/drawing logic

**Implementation context:**
- Canvas background: #000
- Grid: #1d1d1f (barely visible)
- Phase A: #ef4444, B: #22c55e, C: #3b82f6
- Sequence: Pos=#22c55e, Neg=#ef4444, Zero=#3b82f6
- Remove canvas borders

---

### 2.4 Step-by-Step as Drawer

**What it does:** Replace "Show Work" collapsible with a slide-out drawer from the right edge. Feels more intentional than an accordion.

**Why it matters now:** Step-by-step is a key differentiator but it's hidden in a dumpy `<details>` element.

**What it builds on:** Result hero pattern

**What it doesn't touch:** Step-by-step content generation

**Implementation context:**
- Trigger: "See calculation" link below results
- Drawer slides in from right, 50% width on desktop
- Close button or click outside to dismiss
- Smooth transition (300ms ease-out)

---

### 2.5 Tool Switcher

**What it does:** Subtle dropdown or menu to switch between tools without going back to the index. "You're using Fault Current Calculator. Switch to: Mho Visualizer..."

**Why it matters now:** Power users move between tools constantly. Reduce friction.

**What it builds on:** Header from 1.4

**What it doesn't touch:** Individual tool functionality

**Implementation context:**
- Click tool name in header to reveal dropdown
- List all 13 tools with descriptions
- Current tool highlighted
- Close on selection

---

### 2.6 Synchroscope Polish

**What it does:** The synchroscope trainer is the most visual tool. Give it special attention:
- Larger synchroscope dial (fills more of the viewport)
- Waveform visualization below (not beside) on mobile
- Slider controls at bottom, minimal
- "IN SYNC" indicator is the hero moment

**Why it matters now:** This is the tool most likely to be shown to a friend.

**What it builds on:** All Phase 1 + unified diagram styling

**What it doesn't touch:** Animation logic

**Implementation context:**
- Synchroscope: 400px diameter on desktop
- Full-width controls below
- Green glow effect when in sync
- Haptic feedback on mobile when sync achieved (if supported)

---

## Phase 3: Ship This Quarter

### 3.1 Saved Calculations

**What it does:** Users can save a calculation with a name and retrieve it later. Stored in localStorage.

**Why it matters now:** Engineers revisit the same scenarios. Don't make them re-enter.

**What it builds on:** All Phase 1-2 work

**What it doesn't touch:** Calculation logic

**Implementation context:**
- "Save" button appears after calculation
- Name prompt (or auto-generate from inputs)
- "Saved" dropdown in header shows history
- Per-tool storage (fault-current-saved, mho-saved, etc.)

---

### 3.2 Share/Export

**What it does:** Generate a shareable URL with inputs encoded. Also export to PDF.

**Why it matters now:** Engineers share calculations with colleagues. Make it easy.

**What it builds on:** Saved calculations architecture

**What it doesn't touch:** Core functionality

**Implementation context:**
- "Share" button generates URL with query params
- URL loads with inputs pre-filled
- "Export PDF" uses browser print + print stylesheet
- Print stylesheet already partially exists

---

### 3.3 Keyboard Shortcuts

**What it does:** Power users can navigate and operate with keyboard:
- `Tab` through inputs
- `Enter` to calculate (even with auto-calc, confirms intent)
- `S` to save
- `Esc` to close drawers

**Why it matters now:** Differentiator for power users who live in these tools.

**What it builds on:** Polished UI from Phase 2

**What it doesn't touch:** Mouse/touch interaction

**Implementation context:**
- Global keyboard listener
- Visual shortcut hints on hover
- Help modal with shortcut list (`?` to open)

---

### 3.4 Unified Tool Search

**What it does:** From any tool, press `/` to open a search modal. Type to filter tools. Instant navigation.

**Why it matters now:** With 13 tools, finding the right one should be instant.

**What it builds on:** Tool switcher from 2.5

**What it doesn't touch:** Individual tools

**Implementation context:**
- `/` opens modal
- Fuzzy search across tool names and descriptions
- Arrow keys to navigate, Enter to select
- Esc to close

---

### 3.5 Dark/Light Mode Toggle

**What it does:** Some users prefer light mode for printing or well-lit environments. Allow toggle.

**Why it matters now:** Accessibility and preference matter.

**What it builds on:** Design token system

**What it doesn't touch:** Functionality

**Implementation context:**
- Toggle in header (sun/moon icon)
- Swap CSS custom properties
- Persist preference in localStorage
- Default: dark (matches homepage)

---

## Parking Lot

- **Account system** — save calculations to cloud, sync across devices. Too complex for now.
- **AI assistant** — "What fault type should I use?" Natural language input. Requires backend.
- **Unit conversion** — toggle between metric/imperial, per-unit/actual. Adds complexity.
- **Comparison mode** — run same calculation with different inputs side-by-side. Increases cognitive load.
- **API access** — programmatic access to calculations. No demand signal yet.

---

## Rejected Ideas

### "Onboarding tour"
**Considered:** Walk new users through each tool with tooltips.
**Rejected:** If the UI is simple enough, no tour is needed. Tours are band-aids for bad design.

### "Gamification / achievements"
**Considered:** "You've run 100 fault calculations!"
**Rejected:** These are professional tools. Gamification would feel patronizing.

### "Social features / comments"
**Considered:** Let users comment on saved calculations.
**Rejected:** Adds backend complexity for minimal value. Share URLs suffice.

### "Native mobile app"
**Considered:** iOS/Android apps for offline use.
**Rejected:** PWA is sufficient. Web works everywhere.

### "Complex multi-pane layouts"
**Considered:** Dashboard with multiple calculators visible at once.
**Rejected:** Violates simplicity principle. One tool, one focus.

---

## Dependency Map

```
Phase 1 (Foundation)
├── 1.1 Design Tokens ─────────┐
│                              │
├── 1.2 Remove Noise ──────────┼── requires 1.1
│                              │
├── 1.3 Input-First Layout ────┼── requires 1.1, 1.2
│                              │
├── 1.4 Navigation Header ─────┼── requires 1.1
│                              │
└── 1.5 Auto-Calculate ────────┘

Phase 2 (Polish)
├── 2.1 Result Hero ───────────── requires 1.3
├── 2.2 Collapsible Inputs ────── requires 1.3
├── 2.3 Diagram Styling ───────── requires 1.1
├── 2.4 Step-by-Step Drawer ───── requires 2.1
├── 2.5 Tool Switcher ─────────── requires 1.4
└── 2.6 Synchroscope Polish ───── requires 1.*, 2.3

Phase 3 (Features)
├── 3.1 Saved Calculations ────── requires Phase 2
├── 3.2 Share/Export ──────────── requires 3.1
├── 3.3 Keyboard Shortcuts ────── requires Phase 2
├── 3.4 Tool Search ───────────── requires 2.5
└── 3.5 Dark/Light Mode ───────── requires 1.1
```

---

## Success Metrics

- **Reduced time to first calculation** — measure with analytics (if added)
- **Increased return visits** — users come back (saved calculations usage)
- **Share URL usage** — people share their work
- **Mobile usage** — currently likely low due to dense layouts

---

## Approval Request

This plan redesigns the visual layer of 13 tools while preserving all functionality. Each phase is scoped for iterative delivery with explicit checkpoints.

**To proceed:** Reply "proceed" to begin Phase 1.

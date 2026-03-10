# Layouts & UX Reference — Awwwards Skill

## Hero Patterns

### 1. Statement Hero (Most Common Award-Winner)
```
┌─────────────────────────────────────────┐
│                                         │
│     [overline label]                    │
│     MASSIVE DISPLAY                     │
│     HEADLINE                            │
│                                         │
│     Short description text              │
│     [CTA]              [scroll hint]    │
│                                         │
└─────────────────────────────────────────┘
```
- Full viewport height (`100svh`)
- Text takes center stage, no competing visuals
- Minimal elements: title, subtitle, one CTA
- Works for: agencies, portfolios, SaaS landing

### 2. Split Hero
```
┌──────────────────┬──────────────────────┐
│                  │                      │
│  Display Title   │   Visual / 3D /     │
│  Subtitle        │   Illustration       │
│  [CTA]           │                      │
│                  │                      │
└──────────────────┴──────────────────────┘
```
- 50/50 or 40/60 split
- Grid: `grid-template-columns: 1fr 1fr`
- Stack on mobile (text first)
- Works for: product launches, apps, dashboards

### 3. Immersive Media Hero
```
┌─────────────────────────────────────────┐
│ ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │
│ ░░░░░  VIDEO / CANVAS / WEBGL  ░░░░░░ │
│ ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │
│ ░░░░░░  [ Title Overlay ]  ░░░░░░░░░░ │
│ ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │
└─────────────────────────────────────────┘
```
- Full bleed background media
- Text over media with scrim or contrast
- Works for: luxury, creative, entertainment

### 4. Dashboard Hero (for Apps)
```
┌─────────────────────────────────────────┐
│  Welcome back, Thomas          [avatar] │
│                                         │
│  ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐  │
│  │ Stat │ │ Stat │ │ Stat │ │ Stat │  │
│  └──────┘ └──────┘ └──────┘ └──────┘  │
│                                         │
│  ┌─────────────────┐ ┌────────────────┐ │
│  │  Primary Card   │ │  Secondary     │ │
│  │                 │ │                │ │
│  └─────────────────┘ └────────────────┘ │
└─────────────────────────────────────────┘
```
- Personalized greeting, key metrics upfront
- Bento-style card grid below

## Grid Systems

### Bento Grid
```css
.bento {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(180px, auto);
  gap: clamp(12px, 1.5vw, 24px);
}
.bento-wide  { grid-column: span 2; }
.bento-tall  { grid-row: span 2; }
.bento-hero  { grid-column: span 2; grid-row: span 2; }

@media (max-width: 768px) {
  .bento { grid-template-columns: repeat(2, 1fr); }
  .bento-wide, .bento-hero { grid-column: span 2; }
}
```

### Broken Grid (for visual interest)
```css
.broken-grid {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: 24px;
}
.broken-grid .text  { grid-column: 1 / 6; grid-row: 1; align-self: end; }
.broken-grid .image { grid-column: 5 / 13; grid-row: 1; } /* overlap! */
```

### Container + Full Bleed Pattern
```css
.container {
  --max-width: 1200px;
  --padding: clamp(20px, 5vw, 80px);

  width: min(var(--max-width), 100% - var(--padding) * 2);
  margin-inline: auto;
}
.full-bleed {
  width: 100vw;
  margin-left: calc(50% - 50vw);
}
```

## Section Rhythm — Breaking the Template Look

### The Problem
```
[Hero]           ← 120px padding
[Feature 1]      ← 120px padding  ← SAME
[Feature 2]      ← 120px padding  ← SAME = boring
[Feature 3]      ← 120px padding  ← SAME
[CTA]            ← 120px padding
```

### The Solution: Varied Rhythm
```
[Hero — full bleed, 100svh]
                 ← 160px
[Stats row — contained, dense]
                 ← 40px
[Feature — full bleed, split layout]
                 ← 120px
[Testimonials — horizontal scroll, edge-to-edge]
                 ← 80px
[CTA — contained, centered, generous space]
                 ← 200px
```

Rules:
- Alternate section widths (contained ↔ full bleed)
- Vary vertical spacing between sections
- Alternate layout patterns (grid ↔ split ↔ centered ↔ scroll)
- Use background color changes to create visual "rooms"

## Navigation Patterns

### Minimal Sticky Nav (Best for Apps)
```css
.nav {
  position: sticky;
  top: 0;
  z-index: 50;
  backdrop-filter: blur(16px) saturate(180%);
  background: oklch(0.99 0 0 / 0.8);
  border-bottom: 1px solid oklch(0 0 0 / 0.04);
  transition: transform 0.3s cubic-bezier(0.16, 1, 0.3, 1);
}
.nav--hidden { transform: translateY(-100%); } /* hide on scroll down */
```

### Mobile Bottom Nav (App-Style)
```css
.mobile-nav {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 50;
  padding-bottom: env(safe-area-inset-bottom);
  backdrop-filter: blur(20px);
  background: oklch(0.99 0 0 / 0.85);
  border-top: 1px solid oklch(0 0 0 / 0.04);
}
```

## Card Design Patterns

### Elevated Card (Default)
```css
.card {
  background: var(--bg-surface);
  border-radius: 16px;
  border: 1px solid var(--border);
  box-shadow:
    0 1px 2px oklch(0 0 0 / 0.04),
    0 4px 16px oklch(0 0 0 / 0.04);
  transition: box-shadow 0.3s ease, transform 0.3s ease;
}
.card:hover {
  box-shadow:
    0 2px 4px oklch(0 0 0 / 0.04),
    0 8px 32px oklch(0 0 0 / 0.08);
  transform: translateY(-2px);
}
```

### Glass Card
```css
.card-glass {
  background: oklch(1 0 0 / 0.6);
  backdrop-filter: blur(20px) saturate(150%);
  border: 1px solid oklch(1 0 0 / 0.2);
  border-radius: 20px;
}
```

### Data Card (for metrics)
```css
.metric-card {
  padding: clamp(16px, 2vw, 24px);
  border-radius: 12px;
  background: var(--bg-surface);
  border: 1px solid var(--border);
}
.metric-card .value {
  font-family: var(--font-mono);
  font-size: var(--text-h2);
  font-weight: 600;
  letter-spacing: -0.02em;
  font-variant-numeric: tabular-nums;
}
.metric-card .label {
  font-size: var(--text-overline);
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--text-tertiary);
}
```

## Scroll Experiences

### Horizontal Scroll Section
```css
.horizontal-scroll {
  display: flex;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  gap: 24px;
  padding: 24px;
  scrollbar-width: none;
}
.horizontal-scroll::-webkit-scrollbar { display: none; }
.horizontal-scroll > * {
  scroll-snap-align: start;
  flex-shrink: 0;
}
```

### Sticky Section Reveal
```css
.sticky-section {
  position: sticky;
  top: 0;
  height: 100svh;
  display: grid;
  place-items: center;
}
```

## Micro-Interactions

### Button Hover (Premium Feel)
```css
.btn {
  position: relative;
  overflow: hidden;
  transition: color 0.3s ease;
}
.btn::before {
  content: '';
  position: absolute;
  inset: 0;
  background: var(--accent);
  transform: scaleX(0);
  transform-origin: right;
  transition: transform 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}
.btn:hover::before {
  transform: scaleX(1);
  transform-origin: left;
}
```

### Input Focus Animation
```css
.input-wrapper {
  position: relative;
}
.input-wrapper::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 50%;
  width: 0;
  height: 2px;
  background: var(--accent);
  transition: all 0.3s cubic-bezier(0.16, 1, 0.3, 1);
}
.input-wrapper:focus-within::after {
  left: 0;
  width: 100%;
}
```

## Spacing System

```css
:root {
  --space-2xs: clamp(4px, 0.5vw, 6px);
  --space-xs:  clamp(8px, 1vw, 12px);
  --space-sm:  clamp(12px, 1.5vw, 16px);
  --space-md:  clamp(16px, 2vw, 24px);
  --space-lg:  clamp(24px, 3vw, 40px);
  --space-xl:  clamp(40px, 5vw, 64px);
  --space-2xl: clamp(64px, 8vw, 120px);
  --space-3xl: clamp(96px, 12vw, 200px);
}
```

# CSS Techniques Reference — Awwwards Skill

## Scroll-Driven Animations (Native CSS)

### Basic Scroll Progress
```css
@keyframes reveal {
  from { opacity: 0; transform: translateY(30px); }
  to   { opacity: 1; transform: translateY(0); }
}

.scroll-reveal {
  animation: reveal linear both;
  animation-timeline: view();
  animation-range: entry 0% entry 40%;
}
```

### Scroll Progress Bar
```css
.progress-bar {
  position: fixed;
  top: 0;
  left: 0;
  height: 3px;
  background: var(--accent);
  transform-origin: left;
  animation: grow-width linear;
  animation-timeline: scroll(root);
}
@keyframes grow-width {
  from { transform: scaleX(0); }
  to   { transform: scaleX(1); }
}
```

### Parallax with Scroll Timeline
```css
.parallax-bg {
  animation: parallax linear;
  animation-timeline: scroll();
}
@keyframes parallax {
  from { transform: translateY(-20%); }
  to   { transform: translateY(20%); }
}
```

## Container Queries

### Component-Level Responsiveness
```css
.card-wrapper {
  container-type: inline-size;
  container-name: card;
}

.card {
  display: grid;
  grid-template-columns: 1fr;
  gap: 16px;
}

@container card (min-width: 400px) {
  .card {
    grid-template-columns: 120px 1fr;
  }
}

@container card (min-width: 600px) {
  .card {
    grid-template-columns: 200px 1fr auto;
  }
}
```

### Container Query Units
```css
.card-title {
  font-size: clamp(1rem, 3cqi, 1.5rem); /* cqi = container query inline */
}
```

## The :has() Selector

### Parent Styling Based on Children
```css
/* Card with image gets different layout */
.card:has(img) {
  grid-template-rows: 200px 1fr;
}

/* Form group with invalid input */
.form-group:has(:invalid) {
  --border-color: var(--status-negative);
}

/* Nav changes when page has hero */
body:has(.hero) .nav {
  background: transparent;
  position: absolute;
}

/* Sidebar open state */
body:has(.sidebar[data-open="true"]) .main {
  margin-left: 280px;
}
```

## CSS Subgrid

### Aligned Card Contents
```css
.card-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}

.card {
  display: grid;
  grid-template-rows: subgrid;
  grid-row: span 3; /* title, content, footer */
}
```

## @property (CSS Houdini)

### Animated Gradient
```css
@property --gradient-angle {
  syntax: '<angle>';
  initial-value: 0deg;
  inherits: false;
}

.animated-gradient {
  background: conic-gradient(from var(--gradient-angle), #ff0000, #0000ff, #ff0000);
  animation: spin 3s linear infinite;
}

@keyframes spin {
  to { --gradient-angle: 360deg; }
}
```

### Animated Color
```css
@property --glow-color {
  syntax: '<color>';
  initial-value: oklch(0.6 0.2 200);
  inherits: false;
}

.glow {
  box-shadow: 0 0 30px var(--glow-color);
  transition: --glow-color 0.5s ease;
}
.glow:hover {
  --glow-color: oklch(0.7 0.25 300);
}
```

## View Transitions API

### Cross-Document (MPA)
```css
@view-transition { navigation: auto; }

/* Name elements for shared transitions */
.hero-image { view-transition-name: hero; }
.page-title { view-transition-name: title; }

::view-transition-old(hero) {
  animation: scale-down 0.4s var(--ease-in-out);
}
::view-transition-new(hero) {
  animation: scale-up 0.4s var(--ease-in-out);
}
```

### Same-Document (SPA)
```js
document.startViewTransition(() => {
  // Update the DOM
  root.render(<NewPage />);
});
```

## Modern Layout Tricks

### Fluid Spacing with clamp()
```css
section {
  padding-block: clamp(60px, 10vw, 160px);
}
```

### Auto-Fill Responsive Grid (No Media Queries)
```css
.auto-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(min(300px, 100%), 1fr));
  gap: clamp(16px, 2vw, 32px);
}
```

### Aspect Ratio Cards
```css
.card-visual {
  aspect-ratio: 16 / 10;
  overflow: hidden;
  border-radius: 12px;
}
.card-visual img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.6s var(--ease-out);
}
.card:hover .card-visual img {
  transform: scale(1.05);
}
```

### Sticky Sidebar Layout
```css
.layout {
  display: grid;
  grid-template-columns: 280px 1fr;
  gap: 40px;
  align-items: start;
}
.sidebar {
  position: sticky;
  top: 80px;
}
```

## Advanced Selectors

### Quantity Queries
```css
/* If more than 3 items, switch to smaller layout */
.item:nth-last-child(n+4),
.item:nth-last-child(n+4) ~ .item {
  font-size: 0.875rem;
}
```

### Nth-child with selector list
```css
/* Style every 3rd card that has a class of .featured */
.card.featured:nth-child(3n) {
  grid-column: span 2;
}
```

## Performance Patterns

### Content Visibility
```css
.below-fold-section {
  content-visibility: auto;
  contain-intrinsic-size: 0 600px; /* estimated height */
}
```

### Reduce Paint with contain
```css
.card {
  contain: layout style paint;
}
```

### Optimized Backdrop Filter
```css
.glass {
  /* Isolate the blur to prevent expensive full-page compositing */
  isolation: isolate;
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
}
```

## Dark Mode with Color-Mix

```css
:root {
  --text: oklch(0.15 0 0);
  --bg: oklch(0.98 0 0);
}

@media (prefers-color-scheme: dark) {
  :root {
    --text: oklch(0.92 0 0);
    --bg: oklch(0.12 0.01 260);
  }
}

/* Auto-generate hover states */
.btn {
  background: var(--accent);
}
.btn:hover {
  background: color-mix(in oklch, var(--accent), black 15%);
}
```

## Logical Properties

```css
/* Use logical properties for RTL/internationalization */
.card {
  margin-inline: auto;
  padding-block: var(--space-md);
  padding-inline: var(--space-lg);
  border-inline-start: 3px solid var(--accent);
}
```

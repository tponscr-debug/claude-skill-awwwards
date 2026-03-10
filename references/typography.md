# Typography Reference — Awwwards Skill

## Trending Display Fonts (2025-2026)

### Geometric Sans
- **PP Neue Montreal** — Clean, modern, versatile. The new "safe-premium" choice.
- **General Sans** — Open, geometric. Great for tech/SaaS.
- **Satoshi** — Friendly geometric. Good for apps.
- **Cabinet Grotesk** — Strong headlines with character.
- **Clash Display** — Bold, modern, editorial feel.

### Neo-Grotesque
- **Söhne** (Klim) — The modern Helvetica. Used by OpenAI, Stripe.
- **ABC Diatype** — Swiss heritage, tech-friendly.
- **Switzer** — Free, clean alternative to Söhne.
- **Inter** — Best free UI font. Excellent x-height and legibility.

### Serif & Display Serif
- **PP Editorial New** — Dramatic contrast, editorial power.
- **Fraunces** — Variable with optical size axis. Free.
- **Gambetta** — Classic proportions, modern details.
- **Instrument Serif** — Free Google font, elegant for accents.

### Monospace (for data/accents)
- **JetBrains Mono** — Excellent for data-heavy UIs.
- **Berkeley Mono** — Premium developer mono.
- **IBM Plex Mono** — Free, great for labels and stats.
- **Geist Mono** — Vercel's mono, trendy and readable.

## Font Pairing Rules

### The Safe Formula
```
Display: [Geometric Sans — bold/heavy weight]
Body: [Same family or Inter — regular/medium weight]
Accent: [Monospace — for labels, data, tags]
```

### The Editorial Formula
```
Display: [Display Serif — italic or light]
Body: [Neo-Grotesque Sans — regular]
Accent: [Uppercase sans — tracked out, small]
```

### The Tech Formula
```
Display: [Clean Sans — medium/bold]
Body: [Same family — regular]
Data: [Monospace — for metrics, code, numbers]
```

## Fluid Typography Scale

### The Clamp System
```css
:root {
  /* Display / Hero */
  --text-display: clamp(3rem, 8vw + 1rem, 10rem);

  /* Heading levels */
  --text-h1: clamp(2.5rem, 5vw + 1rem, 5rem);
  --text-h2: clamp(1.75rem, 3vw + 0.5rem, 3rem);
  --text-h3: clamp(1.25rem, 2vw + 0.5rem, 1.75rem);

  /* Body */
  --text-body-lg: clamp(1.125rem, 1vw + 0.5rem, 1.375rem);
  --text-body: clamp(1rem, 0.5vw + 0.75rem, 1.125rem);
  --text-body-sm: clamp(0.875rem, 0.5vw + 0.5rem, 0.9375rem);

  /* Labels & Caps */
  --text-label: clamp(0.6875rem, 0.3vw + 0.5rem, 0.8125rem);
  --text-overline: clamp(0.625rem, 0.25vw + 0.5rem, 0.75rem);
}
```

### Tracking (Letter Spacing)
```css
/* Tight for large display text */
.display { letter-spacing: -0.03em; }
.h1      { letter-spacing: -0.025em; }
.h2      { letter-spacing: -0.02em; }

/* Normal for body */
.body    { letter-spacing: -0.011em; }

/* Wide for small uppercase labels */
.label   { letter-spacing: 0.08em; text-transform: uppercase; }
.overline{ letter-spacing: 0.12em; text-transform: uppercase; }
```

### Line Height
```css
.display { line-height: 0.9; }    /* Very tight for huge text */
.h1      { line-height: 0.95; }
.h2      { line-height: 1.1; }
.h3      { line-height: 1.2; }
.body    { line-height: 1.55; }   /* Comfortable reading */
.label   { line-height: 1.2; }
```

## Text Animation Techniques

### Split Text Reveal (GSAP)
```js
import { gsap } from 'gsap';
import SplitType from 'split-type';

const text = new SplitType('.hero-title', { types: 'chars, words' });

gsap.from(text.chars, {
  y: '100%',
  opacity: 0,
  duration: 0.8,
  ease: 'power4.out',
  stagger: 0.02,
});
```

### CSS-Only Line Reveal
```css
.line-reveal {
  overflow: hidden;
}
.line-reveal span {
  display: inline-block;
  transform: translateY(110%);
  animation: lineUp 0.6s cubic-bezier(0.16, 1, 0.3, 1) forwards;
}
@keyframes lineUp {
  to { transform: translateY(0); }
}
```

### Counter Animation (for metrics/stats)
```js
function animateCounter(el, target, duration = 1500) {
  const start = performance.now();
  const from = 0;

  function tick(now) {
    const t = Math.min((now - start) / duration, 1);
    const ease = 1 - Math.pow(1 - t, 4); // power4.out
    el.textContent = Math.round(from + (target - from) * ease);
    if (t < 1) requestAnimationFrame(tick);
  }
  requestAnimationFrame(tick);
}
```

## Responsive Typography Tips

- **Never** use `px` for font sizes in production — use `rem` or `clamp()`
- **Limit line length** to 60-75 characters (`max-width: 65ch`)
- **Reduce hero size** on mobile by ~40% (the `clamp()` handles this automatically)
- **Increase body line-height** on mobile to `1.6-1.7`
- **Use `text-wrap: balance`** on headings for better line breaks
- **Use `text-wrap: pretty`** on body paragraphs to avoid orphans

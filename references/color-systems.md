# Color Systems Reference — Awwwards Skill

## Core Rules

1. **Never pure black (#000) or pure white (#fff)** for backgrounds
2. **Max 1-2 accent colors** — restraint is sophistication
3. **4+ text gray levels** — primary, secondary, tertiary, disabled
4. **Use oklch()** for perceptually uniform color manipulation
5. **Borders use rgba/hsla** with very low opacity (0.04-0.12)

## Archetype Palettes

### Dark Immersive (Sports, Gaming, Creative Tech)
```css
:root {
  --bg-primary: oklch(0.13 0.005 260);    /* near-black with blue tint */
  --bg-surface: oklch(0.17 0.008 260);    /* card surface */
  --bg-elevated: oklch(0.21 0.01 260);    /* popover, modal */
  --text-primary: oklch(0.95 0.005 260);  /* off-white */
  --text-secondary: oklch(0.65 0.01 260); /* muted */
  --text-tertiary: oklch(0.45 0.01 260);  /* subtle */
  --accent: oklch(0.72 0.19 145);         /* electric green */
  --accent-muted: oklch(0.72 0.19 145 / 0.15);
  --border: oklch(0.95 0 0 / 0.08);
}
```

### Warm Editorial (Magazine, Luxury, Food, Lifestyle)
```css
:root {
  --bg-primary: oklch(0.97 0.005 80);     /* warm cream */
  --bg-surface: oklch(0.99 0.002 80);     /* near-white */
  --bg-elevated: oklch(1.0 0 0);
  --text-primary: oklch(0.15 0.01 50);    /* warm black */
  --text-secondary: oklch(0.45 0.02 50);  /* warm gray */
  --text-tertiary: oklch(0.6 0.02 50);
  --accent: oklch(0.55 0.15 25);          /* terracotta */
  --accent-muted: oklch(0.55 0.15 25 / 0.12);
  --border: oklch(0.15 0 0 / 0.06);
}
```

### Monochrome + Pop (SaaS, Fintech, Developer Tools)
```css
:root {
  --bg-primary: oklch(0.985 0 0);         /* near-white */
  --bg-surface: oklch(1.0 0 0);
  --bg-elevated: oklch(0.97 0 0);
  --text-primary: oklch(0.13 0 0);        /* near-black */
  --text-secondary: oklch(0.45 0 0);
  --text-tertiary: oklch(0.65 0 0);
  --accent: oklch(0.63 0.26 29);          /* vivid orange-red */
  --accent-muted: oklch(0.63 0.26 29 / 0.1);
  --border: oklch(0.13 0 0 / 0.06);
}
```

### Gradient Universe (AI, Crypto, Creative Agency)
```css
:root {
  --bg-primary: oklch(0.12 0.02 280);
  --bg-surface: oklch(0.16 0.02 280);
  --text-primary: oklch(0.95 0 0);
  --text-secondary: oklch(0.7 0.02 280);
  --gradient-hero: linear-gradient(135deg,
    oklch(0.6 0.25 300),
    oklch(0.65 0.2 230),
    oklch(0.7 0.22 180));
  --border: oklch(0.95 0 0 / 0.06);
}
```

### Pastel Soft (Health, Wellness, Kids, Lifestyle Apps)
```css
:root {
  --bg-primary: oklch(0.97 0.01 230);     /* soft blue tint */
  --bg-surface: oklch(0.99 0.005 230);
  --text-primary: oklch(0.2 0.02 260);
  --text-secondary: oklch(0.5 0.02 260);
  --accent: oklch(0.7 0.15 160);          /* soft teal */
  --accent-secondary: oklch(0.75 0.12 330); /* soft pink */
  --border: oklch(0.2 0 0 / 0.05);
}
```

## Sport-Specific Color System

### Training Zone Colors (Physiologically Accurate)
```css
/* These map to heart rate / lactate zones */
--zone-recovery: oklch(0.7 0.14 230);    /* cool blue — rest */
--zone-easy: oklch(0.72 0.16 160);       /* teal-green — aerobic base */
--zone-tempo: oklch(0.75 0.16 85);       /* gold/amber — threshold */
--zone-interval: oklch(0.65 0.22 30);    /* deep orange — VO2max */
--zone-sprint: oklch(0.6 0.24 15);       /* red — anaerobic */
```

### Status & Feedback Colors
```css
--status-positive: oklch(0.72 0.17 155); /* green — good form, PR, improvement */
--status-warning: oklch(0.78 0.16 80);   /* amber — overtraining risk */
--status-negative: oklch(0.65 0.2 25);   /* red — injury risk, overreaching */
--status-neutral: oklch(0.6 0.02 260);   /* gray-blue — informational */
```

## oklch() Techniques

### Generate Harmonious Palettes
```css
/* Same lightness & chroma, vary hue for harmony */
--color-1: oklch(0.7 0.15 30);   /* base */
--color-2: oklch(0.7 0.15 90);   /* +60° analogous */
--color-3: oklch(0.7 0.15 210);  /* complementary */
```

### Darken/Lighten Without Mudding
```css
/* oklch lets you adjust lightness linearly */
--accent: oklch(0.65 0.2 145);
--accent-light: oklch(0.80 0.15 145);  /* lighter, slightly less saturated */
--accent-dark: oklch(0.50 0.2 145);    /* darker, same saturation */
--accent-bg: oklch(0.65 0.2 145 / 0.08); /* tint for backgrounds */
```

## Gradient Recipes

### Subtle Mesh Background
```css
.mesh-bg {
  background:
    radial-gradient(ellipse at 20% 50%, oklch(0.7 0.1 200 / 0.15), transparent 50%),
    radial-gradient(ellipse at 80% 20%, oklch(0.7 0.12 300 / 0.12), transparent 50%),
    radial-gradient(ellipse at 50% 80%, oklch(0.7 0.08 150 / 0.1), transparent 50%),
    oklch(0.985 0 0);
}
```

### Text Gradient
```css
.gradient-text {
  background: linear-gradient(135deg, oklch(0.6 0.25 300), oklch(0.7 0.2 200));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}
```

### Animated Gradient Border
```css
.gradient-border {
  position: relative;
  border-radius: 16px;
  padding: 1px;
  background: linear-gradient(var(--angle, 0deg), oklch(0.6 0.2 300), oklch(0.7 0.2 200));
  animation: rotate-gradient 4s linear infinite;
}
.gradient-border > * {
  background: var(--bg-surface);
  border-radius: 15px;
}
@property --angle {
  syntax: '<angle>';
  initial-value: 0deg;
  inherits: false;
}
@keyframes rotate-gradient {
  to { --angle: 360deg; }
}
```

## Dark Mode Strategy

### Approach: Semantic Tokens
```css
/* Light mode — default */
:root {
  --bg-primary: oklch(0.985 0.005 80);
  --text-primary: oklch(0.13 0.01 50);
  --border: oklch(0 0 0 / 0.06);
}

/* Dark mode */
@media (prefers-color-scheme: dark) {
  :root {
    --bg-primary: oklch(0.13 0.01 260);
    --text-primary: oklch(0.93 0.005 260);
    --border: oklch(1 0 0 / 0.08);
  }
}
```

### Key Dark Mode Rules
- Don't just invert — dark backgrounds should have a subtle hue (blue, warm, etc.)
- Reduce chroma on accent colors (vivid on dark = harsh)
- Elevations get LIGHTER (opposite of light mode shadows)
- Borders switch from dark-on-light to light-on-dark (adjust opacity)
- Images/illustrations may need reduced brightness: `filter: brightness(0.85)`

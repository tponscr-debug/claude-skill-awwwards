# Motion Design Reference — Awwwards Skill

## Easing Library

```css
:root {
  /* Standard — use for most UI transitions */
  --ease-out: cubic-bezier(0.16, 1, 0.3, 1);        /* power4.out */
  --ease-in-out: cubic-bezier(0.76, 0, 0.24, 1);     /* power4.inOut */
  --ease-in: cubic-bezier(0.55, 0, 1, 0.45);          /* power3.in */

  /* Bouncy / Elastic */
  --ease-back: cubic-bezier(0.34, 1.56, 0.64, 1);    /* back.out */
  --ease-spring: cubic-bezier(0.22, 1.36, 0.36, 1);   /* spring-like */

  /* Dramatic — use for page transitions & hero animations */
  --ease-expo: cubic-bezier(0.16, 1, 0.3, 1);         /* expo.out */
  --ease-expo-in-out: cubic-bezier(0.87, 0, 0.13, 1); /* expo.inOut */
}
```

## Duration Scale

| Category | Duration | Use Case |
|----------|----------|----------|
| Micro | 100-150ms | Button state, toggle, icon swap |
| Small | 200-300ms | Hover effects, tooltip, dropdown |
| Medium | 400-500ms | Modal open, card reveal, tab switch |
| Large | 600-800ms | Scroll reveal, section transition |
| XL | 1000-1500ms | Hero entrance, page transition |

**Rule:** The larger the element moving, the longer the duration.

## GSAP Patterns

### Setup
```bash
npm install gsap @gsap/react
```

### Scroll-Triggered Reveal
```js
import { gsap } from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';
import { useGSAP } from '@gsap/react';

gsap.registerPlugin(ScrollTrigger);

function Section() {
  const containerRef = useRef(null);

  useGSAP(() => {
    const elements = containerRef.current.querySelectorAll('.reveal');

    gsap.from(elements, {
      y: 40,
      opacity: 0,
      duration: 0.8,
      ease: 'power4.out',
      stagger: 0.08,
      scrollTrigger: {
        trigger: containerRef.current,
        start: 'top 80%',
        once: true,
      },
    });
  }, { scope: containerRef });

  return (
    <section ref={containerRef}>
      <h2 className="reveal">Title</h2>
      <p className="reveal">Description</p>
    </section>
  );
}
```

### Number Counter
```js
useGSAP(() => {
  const obj = { val: 0 };

  gsap.to(obj, {
    val: targetValue,
    duration: 1.5,
    ease: 'power4.out',
    scrollTrigger: {
      trigger: counterRef.current,
      start: 'top 85%',
      once: true,
    },
    onUpdate: () => {
      counterRef.current.textContent = Math.round(obj.val);
    },
  });
});
```

### Staggered Card Grid
```js
useGSAP(() => {
  gsap.from('.card', {
    y: 60,
    opacity: 0,
    duration: 0.7,
    ease: 'power4.out',
    stagger: {
      amount: 0.4,        // total stagger time
      grid: [3, 3],        // grid awareness
      from: 'start',
    },
    scrollTrigger: {
      trigger: '.card-grid',
      start: 'top 75%',
      once: true,
    },
  });
});
```

### Horizontal Scroll Section
```js
useGSAP(() => {
  const container = scrollRef.current;
  const scrollWidth = container.scrollWidth - container.clientWidth;

  gsap.to(container, {
    scrollLeft: scrollWidth,
    ease: 'none',
    scrollTrigger: {
      trigger: container,
      start: 'top top',
      end: () => `+=${scrollWidth}`,
      scrub: 1,
      pin: true,
    },
  });
});
```

### Smooth Scroll with Lenis
```bash
npm install lenis
```
```js
import Lenis from 'lenis';

useEffect(() => {
  const lenis = new Lenis({
    lerp: 0.1,
    smoothWheel: true,
  });

  function raf(time) {
    lenis.raf(time);
    requestAnimationFrame(raf);
  }
  requestAnimationFrame(raf);

  // Connect to GSAP ScrollTrigger
  lenis.on('scroll', ScrollTrigger.update);
  gsap.ticker.add((time) => lenis.raf(time * 1000));
  gsap.ticker.lagSmoothing(0);

  return () => lenis.destroy();
}, []);
```

## Page Transitions

### View Transitions API (Modern Browsers)
```js
// Navigate with transition
function navigate(href) {
  if (!document.startViewTransition) {
    window.location.href = href;
    return;
  }

  document.startViewTransition(() => {
    // update DOM here (React router handles this)
  });
}
```
```css
::view-transition-old(root) {
  animation: fade-out 0.3s var(--ease-in-out);
}
::view-transition-new(root) {
  animation: fade-in 0.3s var(--ease-in-out);
}
```

### SPA Page Transition (React)
```js
// Wrap route content with AnimatePresence (Framer Motion) or custom
function PageTransition({ children }) {
  const [isVisible, setIsVisible] = useState(false);

  useEffect(() => {
    requestAnimationFrame(() => setIsVisible(true));
    return () => setIsVisible(false);
  }, []);

  return (
    <div className={`page-transition ${isVisible ? 'enter' : 'exit'}`}>
      {children}
    </div>
  );
}
```
```css
.page-transition {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.4s var(--ease-out), transform 0.4s var(--ease-out);
}
.page-transition.enter {
  opacity: 1;
  transform: translateY(0);
}
```

## Cursor Effects

### Custom Cursor (Desktop Only)
```js
function CustomCursor() {
  const cursorRef = useRef(null);

  useEffect(() => {
    if (window.matchMedia('(pointer: coarse)').matches) return; // skip touch

    const cursor = cursorRef.current;
    let x = 0, y = 0, cx = 0, cy = 0;

    document.addEventListener('mousemove', (e) => {
      x = e.clientX;
      y = e.clientY;
    });

    function animate() {
      cx += (x - cx) * 0.15; // lerp for smooth follow
      cy += (y - cy) * 0.15;
      cursor.style.transform = `translate(${cx}px, ${cy}px)`;
      requestAnimationFrame(animate);
    }
    animate();
  }, []);

  return <div ref={cursorRef} className="custom-cursor" />;
}
```

## Loading Sequences

### Loader → Content Reveal
```js
function Loader({ onComplete }) {
  useGSAP(() => {
    const tl = gsap.timeline({ onComplete });

    tl.to('.loader-bar', {
      scaleX: 1,
      duration: 1.5,
      ease: 'power2.inOut',
    })
    .to('.loader', {
      yPercent: -100,
      duration: 0.8,
      ease: 'power4.inOut',
    })
    .from('.hero-content > *', {
      y: 40,
      opacity: 0,
      duration: 0.6,
      stagger: 0.1,
      ease: 'power4.out',
    }, '-=0.3');
  });

  return (
    <div className="loader">
      <div className="loader-bar" />
    </div>
  );
}
```

## Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```
```js
// In JS, check before animating
const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

if (prefersReducedMotion) {
  gsap.set(elements, { opacity: 1, y: 0 }); // skip animation, show final state
} else {
  gsap.from(elements, { opacity: 0, y: 40, duration: 0.8 });
}
```

## Performance Rules

1. **Animate only `transform` and `opacity`** — these are GPU-composited
2. **Never animate `width`, `height`, `top`, `left`** — triggers layout
3. **Use `will-change: transform`** sparingly on elements about to animate
4. **Avoid animating more than 30 elements simultaneously**
5. **Use `ScrollTrigger.batch()`** for large lists instead of individual triggers
6. **Lazy-load GSAP plugins** — only import what you use
7. **Kill ScrollTriggers on unmount** — memory leaks are real

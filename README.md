# /awwwards — Elite Creative Direction Skill for Claude Code

A Claude Code custom skill that transforms your frontend/UI work to Awwwards Site of the Day level (8.0+ score).

## What it does

When invoked (manually with `/awwwards` or auto-detected on frontend work), Claude Code operates as a Senior Creative Director with expertise in:

- **Typography** — Fluid `clamp()` scales, trending font pairings, kinetic text animation
- **Color Systems** — Perceptually uniform palettes, dark mode, gradient recipes
- **Layouts** — Bento grids, broken grids, varied section rhythm, hero patterns
- **Motion Design** — GSAP patterns, scroll reveals, page transitions, cursor effects
- **CSS Techniques** — Scroll-driven animations, container queries, `:has()`, View Transitions
- **WebGL / 3D** — Three.js, React Three Fiber, shaders, particles, performance budgets
- **Studio Philosophies** — Awwwards judging criteria, industry-specific design languages, 2025-26 trends

## Install

```bash
git clone git@github.com:tponscr-debug/claude-skill-awwwards.git ~/.claude/skills/awwwards
```

That's it. Claude Code auto-discovers skills in `~/.claude/skills/`.

## Recommended CLAUDE.md addition

Add this to your `~/.claude/CLAUDE.md` (or project `.claude/CLAUDE.md`) to ensure Claude always uses the skill for frontend work:

```markdown
- Design Standard — Use the `/awwwards` skill: For ANY frontend/UI work, invoke the `/awwwards` skill which contains the complete creative direction framework, design decision process, and links to 7 reference files covering typography, color, layout, motion, CSS techniques, WebGL, and studio philosophies. Key principles:
  - ALWAYS define art direction BEFORE writing code (emotion, archetype, industry context)
  - Typography is 90% of design — use fluid `clamp()` scales, max 2 font families, tight tracking on display
  - Never pure black/white — use oklch() or carefully chosen near-black/near-white values
  - Motion must tell a story — every animation answers "what happened?" or "where should I look?"
  - Vary section rhythm — alternate contained/full-bleed, vary spacing, break the template look
  - Performance IS design — target <3s load, 60fps, animate only `transform` and `opacity`
  - A generic, template-looking UI is a failure. The user must feel the product is one-of-a-kind on first load.
```

## Usage

### Manual
Type `/awwwards` in Claude Code to activate creative direction mode.

### Automatic
The skill auto-triggers on frontend design, UI/UX work, styling, and CSS tasks (via the `description` field in `SKILL.md` frontmatter).

### In conversation
Ask Claude to design or redesign any UI — it will consult the reference files, define art direction (emotion, archetype, typography, color, motion), then build.

## File structure

```
~/.claude/skills/awwwards/
├── SKILL.md                         # Main prompt (auto-loaded by Claude Code)
└── references/
    ├── typography.md                # Fonts, pairings, fluid type, text animation
    ├── color-systems.md             # Palettes by archetype, oklch, gradients, dark mode
    ├── layouts-ux.md                # Grids, heroes, cards, micro-interactions, spacing
    ├── motion-design.md             # GSAP, Lenis, page transitions, cursors, loaders
    ├── css-techniques.md            # Scroll-driven animations, container queries, :has()
    ├── webgl-3d.md                  # Three.js, R3F, shaders, particles, performance
    └── studios-philosophy.md        # Awwwards criteria, elite studios, industry design
```

## License

MIT — Use it, fork it, make beautiful things.

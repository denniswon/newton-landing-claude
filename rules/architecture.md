# Architecture Guidelines

## Project Structure

```
src/
├── app/
│   ├── layout.tsx            # Root layout: fonts, ThemeProvider, Header, Footer, GA
│   ├── page.tsx              # Page orchestrator: static + lazy-loaded sections
│   ├── site.config.ts        # Single source of truth for all content
│   ├── globals.css           # Tailwind 4 config, theme tokens, utility classes
│   ├── api/whitepaper/       # Email-gated PDF download (Loops.so CRM)
│   └── whitepaper/           # Whitepaper download page
├── components/
│   ├── sections/             # One file per page section, top-to-bottom order
│   ├── ui/                   # Reusable primitives (NavButton, ExternalLink, etc.)
│   ├── ThemeProvider.tsx      # Dark/light theme context + localStorage persistence
│   └── SmoothScroll.tsx       # Lenis smooth scroll initialization
└── fonts/                    # GT Sectra Fine woff2 variants
```

## Content System

All page content lives in `src/app/site.config.ts`. This is the **single source of truth** for every string, link, asset path, and CTA on the site. Section components destructure their slice from `siteConfig` — they never hardcode strings.

```typescript
// In a section component
const { hero } = siteConfig
return (
  <section>
    <h1>{hero.title}</h1>
    <p>{hero.subtitle}</p>
  </section>
)
```

When adding content:
1. Add the data to `site.config.ts` first
2. Type the shape if new
3. Consume from the component via destructuring

## Theme System (3-Layer)

### Layer 1: Inline Script (No Flash)

An inline `<script>` in `<head>` reads `localStorage` and sets `data-theme` on `<html>` before paint. This prevents the flash of wrong theme.

### Layer 2: ThemeProvider (React State)

`ThemeProvider` syncs React state from the DOM `data-theme` attribute on hydration. Exposes `theme` and `toggleTheme` via context.

### Layer 3: CSS Variables

`globals.css` defines semantic tokens that switch based on `[data-theme]`:

```css
[data-theme="light"] {
  --color-on-surface: #1a1a1a;
  --color-surface: #ffffff;
  --color-surface-alt: #f5f5f5;
}

[data-theme="dark"] {
  --color-on-surface: #f5f5f5;
  --color-surface: #0a0a0a;
  --color-surface-alt: #1a1a1a;
}
```

Semantic tokens (`text-on-surface`, `bg-surface-alt`, etc.) are used throughout — never raw hex values in JSX.

## Loading Strategy

### Static Imports (Above Fold)

First 4 sections are statically imported for fast LCP:
- AnnouncementBanner
- HeroSection
- ValuePropSection
- LogoStripSection

### Lazy Loading (Below Fold)

Remaining 8 sections use `next/dynamic` for lazy loading:

```typescript
const SolutionSection = dynamic(() =>
  import('@/components/sections/SolutionSection').then(m => ({ default: m.SolutionSection }))
)
```

## Animation Strategy

### Motion (Framer Motion 12)

- Scroll-driven animations via `useInView` and `useScroll`
- Spring physics for natural feel
- `AnimatePresence` for enter/exit transitions
- Prefer `motion.div` over manual keyframe CSS

### Lenis (Smooth Scroll)

- Duration: 1.2s
- Touch: disabled (native scroll on mobile)
- Initialized in `SmoothScroll.tsx` client component

### PhysicsMarquee

Custom scroll-linked marquee with spring-animated spotlight effect. Used in the logo strip section.

## Component Patterns

### Section Components

Each section is a self-contained component in `src/components/sections/`:

```typescript
export function ValuePropSection() {
  const { valueProp } = siteConfig

  return (
    <section id="value-prop" className="py-20 md:py-32">
      {/* Section content using valueProp data */}
    </section>
  )
}
```

Rules:
- One component per file
- Named export (no default)
- Content from `siteConfig` only
- `id` attribute for anchor navigation
- Responsive padding: mobile-first

### UI Primitives

Reusable components in `src/components/ui/`:

- `NavButton` — navigation buttons with consistent styling
- `ExternalLink` — auto-adds `target="_blank" rel="noopener noreferrer"`
- Use `ExternalLink` for ALL external hrefs

### External Links

Always use the `ExternalLink` component for external URLs:

```typescript
// Good
<ExternalLink href="https://docs.newton.xyz">Documentation</ExternalLink>

// Bad
<a href="https://docs.newton.xyz" target="_blank">Documentation</a>
```

## Environment Variables

| Variable | Scope | Purpose |
|----------|-------|---------|
| `NEXT_PUBLIC_SITE_URL` | Client | Site URL (defaults to `https://newton.xyz`) |
| `LOOPS_API_KEY` | Server only | Loops.so CRM API key for whitepaper email capture |

`LOOPS_API_KEY` must never appear in client-side code or be prefixed with `NEXT_PUBLIC_`.

## Design Principles

### Content-Driven

The site is content-driven. `site.config.ts` is the single source of truth. Components render content — they don't define it.

### Performance-First

- Lazy load below-fold sections
- Optimize images with `next/image`
- Preload fonts
- Minimize client-side JavaScript

### Accessible

- Semantic HTML (`<section>`, `<nav>`, `<main>`, `<footer>`)
- Keyboard navigation
- Proper ARIA attributes
- Color contrast meets WCAG AA

### Mobile-First Responsive

- Base styles target mobile
- `md:` breakpoint for tablet+
- `lg:` breakpoint for desktop side-by-side layouts

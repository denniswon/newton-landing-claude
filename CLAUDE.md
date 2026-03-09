# Newton Landing Page — Agent Guidelines

## Project Overview

The newton.xyz landing page — a Next.js marketing site for Newton Protocol. Deployed on Vercel, built with React 19, Tailwind CSS 4, and Motion 12 (Framer Motion). The site targets institutions, enterprises, fintech companies, and web3-native partners evaluating Newton Protocol for compliance, authorization, and risk management infrastructure.

## Tech Stack

| Component       | Technology                          |
|-----------------|-------------------------------------|
| Framework       | Next.js 16.x (App Router)          |
| Language        | TypeScript (strict mode)            |
| Styling         | Tailwind CSS 4 (inline `@theme`)    |
| Animation       | Motion 12 (formerly Framer Motion)  |
| Smooth Scroll   | Lenis 1.3                           |
| Package Manager | npm                                 |
| Linting         | ESLint 9 (`eslint-config-next`)     |
| Unit Tests      | Jest 30 + React Testing Library 16  |
| E2E Tests       | Playwright 1.58 (Chromium)          |
| Hosting         | Vercel                              |
| Analytics       | Google Analytics (GA4)              |
| Node            | >= 20                               |

## Architecture

### Content System

All page content lives in a single file: `src/app/site.config.ts`. This is the single source of truth for every string, link, asset path, and CTA on the site. Section components destructure their slice from `siteConfig` — they never hardcode strings.

### Component Organization

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

### Theme System (3-Layer)

1. **Inline `<script>`** in `<head>` reads localStorage and sets `data-theme` before paint (prevents flash)
2. **`ThemeProvider`** syncs React state from DOM attribute on hydration
3. **CSS variables** in `globals.css` switch semantic tokens based on `[data-theme]`

Semantic tokens (`text-on-surface`, `bg-surface-alt`, etc.) are used throughout — never raw hex values in JSX.

### Loading Strategy

First 4 sections (AnnouncementBanner, HeroSection, ValuePropSection, LogoStripSection) are statically imported for fast LCP. Remaining 8 sections use `next/dynamic` for lazy loading.

### Animation Strategy

- **Motion** — scroll-driven animations, spring physics, AnimatePresence for enter/exit
- **Lenis** — smooth scroll (duration 1.2s, touch disabled)
- **PhysicsMarquee** — custom scroll-linked marquee with spring-animated spotlight effect

## Essential Commands

| Command | Purpose |
|---------|---------|
| `npm run dev` | Local dev server at localhost:3000 |
| `npm run build` | Production build |
| `npm run lint` | ESLint |
| `npx tsc --noEmit` | Type checking |
| `npm test` | Jest unit tests |
| `npm run test:e2e` | Playwright E2E tests |

Always verify with: `npx tsc --noEmit && npm run lint && npm test && npm run build`

## Environment Variables

| Variable | Scope | Purpose |
|----------|-------|---------|
| `NEXT_PUBLIC_SITE_URL` | Client | Site URL (defaults to `https://newton.xyz`) |
| `LOOPS_API_KEY` | Server only | Loops.so CRM API key for whitepaper email capture |

## Key Design Conventions

- `rounded-[2px]` on all interactive elements (deliberate design choice)
- Semantic color tokens only — no raw hex in JSX
- `ExternalLink` component for all external hrefs (auto-adds `target="_blank" rel="noopener noreferrer"`)
- Mobile-first responsive: `md:` for tablet+, `lg:` for desktop side-by-side
- No `cn()` or `clsx` — plain template literals for conditional classes
- No barrel `index.ts` files — direct file path imports
- SVG icons inline in JSX for UI icons, `next/image` from `public/assets/` for brand assets

## Key Principles

1. **Content via `site.config.ts` only** — never hardcode strings in components
2. **No secrets in client bundle** — `LOOPS_API_KEY` is server-side only
3. **Semantic tokens, not raw values** — use theme tokens for all colors
4. **Performance-first** — lazy load below-fold sections, optimize images, preload fonts
5. **Accessible** — semantic HTML, keyboard navigation, proper ARIA attributes
6. **Test everything** — unit tests for components, E2E for user flows, config integrity tests

## Modular Rules

Detailed guidelines are in `.claude/rules/`:

| File | Contents |
|------|----------|
| `communication-style.md` | PR review tone, comment conventions, Notion thread style |
| `code-style.md` | TypeScript/React/Tailwind conventions, naming, anti-patterns |
| `architecture.md` | Content system, component patterns, theme, animation |
| `testing.md` | Jest + Playwright patterns, what to test |
| `security.md` | Client-side security, env vars, external links |
| `performance.md` | Core Web Vitals, lazy loading, image/font optimization |
| `newton-knowledge.md` | Newton Protocol positioning, use cases, target audience |
| `lessons.md` | Recurring mistakes and prevention rules |
| `docs-sync.md` | Deduplication rules for `.claude/` maintenance |

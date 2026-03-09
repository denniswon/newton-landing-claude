# Performance Guidelines

## Core Web Vitals Targets

| Metric | Target | Strategy |
|--------|--------|----------|
| LCP | < 2.5s | Static import above-fold sections, preload fonts, optimize hero image |
| FID/INP | < 100ms | Minimal client-side JS, lazy load interactive sections |
| CLS | < 0.1 | Reserve space for images, no layout shifts from fonts or theme |

## Loading Strategy

### Above-Fold (Static Import)

First 4 sections are statically imported — they render immediately with the initial bundle:
- AnnouncementBanner
- HeroSection
- ValuePropSection
- LogoStripSection

### Below-Fold (Dynamic Import)

Remaining sections use `next/dynamic` — they load only when needed:

```typescript
const SolutionSection = dynamic(() =>
  import('@/components/sections/SolutionSection').then(m => ({ default: m.SolutionSection }))
)
```

Do not move sections between static and dynamic without measuring the impact on LCP.

## Image Optimization

### next/image

Use `next/image` for all images from `public/assets/`:

```typescript
import Image from 'next/image'

<Image
  src="/assets/hero-illustration.webp"
  alt="Newton Protocol architecture"
  width={800}
  height={600}
  priority  // Only for above-fold images
/>
```

Rules:
- `priority` attribute only on above-fold images (hero, logo strip)
- Always provide `width` and `height` to prevent CLS
- Use WebP format when possible
- Provide descriptive `alt` text

### SVG Icons

Inline SVGs for UI icons (small, no network request). `next/image` for brand assets and illustrations.

## Font Optimization

### GT Sectra Fine

Custom font loaded from `src/fonts/`:
- Use `next/font/local` for self-hosting
- `font-display: swap` to prevent FOIT
- Preload the primary weight (regular) used above the fold
- Subset to Latin characters if possible

## JavaScript Optimization

### Client Components

Minimize `'use client'` directives. Each client component adds to the interactive bundle:
- Keep server components as the default
- Extract only the interactive parts into client components
- Avoid wrapping entire sections in `'use client'` for a single click handler

### Animation Budget

Motion (Framer Motion) is the largest client-side dependency. Optimize:
- Use CSS animations for simple transitions (opacity, transform)
- Reserve Motion for scroll-driven animations and spring physics
- `AnimatePresence` only when enter/exit transitions are needed
- Avoid animating layout properties (`width`, `height`) — use `transform` and `opacity`

### Third-Party Scripts

Google Analytics is loaded via Next.js `<Script>` component with `strategy="afterInteractive"`:
- Never block rendering for analytics
- No other third-party scripts unless absolutely necessary

## CSS Optimization

### Tailwind CSS 4

Tailwind 4 uses the inline `@theme` directive in `globals.css` — no separate `tailwind.config.js`:
- Utility classes are tree-shaken automatically
- Custom theme tokens defined inline
- No unused CSS in production

### No Runtime CSS-in-JS

Do not add styled-components, Emotion, or other runtime CSS-in-JS libraries. Tailwind handles all styling.

## Build Verification

After changes, verify build output:

```bash
npm run build
```

Check for:
- No unexpected increase in bundle size
- No hydration warnings
- All pages pre-rendered successfully

## Performance Monitoring

- Google Analytics (GA4) tracks Core Web Vitals
- Vercel Analytics (if enabled) provides per-deployment metrics
- Test with Lighthouse before merging significant UI changes

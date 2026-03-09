# Code Style Guidelines

## Formatting: ESLint

Configured via `eslint-config-next`. Run with `npm run lint`.

## Naming Conventions

| Type | Convention | Example |
|------|------------|---------|
| Components | PascalCase | `HeroSection`, `NavButton` |
| Functions | camelCase | `handleScroll`, `toggleTheme` |
| Variables | camelCase | `isMenuOpen`, `sectionRef` |
| Constants | SCREAMING_SNAKE_CASE | `GA_MEASUREMENT_ID` |
| Types/Interfaces | PascalCase | `SiteConfig`, `NavItem` |
| Files (components) | PascalCase | `HeroSection.tsx`, `NavButton.tsx` |
| Files (utilities) | camelCase | `useTheme.ts` |
| CSS variables | kebab-case | `--color-on-surface` |
| Test files | PascalCase with `.test` | `HeroSection.test.tsx` |

## TypeScript Patterns

### Strict Mode

All code runs with `strict: true`. No `any` unless absolutely necessary and documented.

### No Default Exports (Exception: Pages)

Use named exports for components and utilities. Next.js pages/layouts are the only exception (framework requirement).

```typescript
// Good: Named export for components
export function HeroSection() { ... }

// Acceptable: Default export for Next.js pages
export default function Page() { ... }
```

### Props Types

Define props inline or as named types — no `interface` unless extending:

```typescript
// Good: Inline for simple props
function NavButton({ href, children }: { href: string; children: React.ReactNode }) { ... }

// Good: Named type for complex props
type SectionProps = {
  id: string
  title: string
  description: string
  cta?: { label: string; href: string }
}
```

## React / Next.js Patterns

### Server vs Client Components

Default to server components. Only add `'use client'` when the component needs:
- Event handlers (onClick, onChange, etc.)
- Hooks (useState, useEffect, useRef, etc.)
- Browser APIs (window, document, localStorage)
- Motion/Framer Motion animations

### Content from site.config.ts

Components destructure their content from `siteConfig` — never hardcode strings:

```typescript
// Good
const { hero } = siteConfig
return <h1>{hero.title}</h1>

// Bad
return <h1>Newton Protocol</h1>
```

### Conditional Classes

Use plain template literals — no `cn()` or `clsx`:

```typescript
// Good
className={`text-on-surface ${isActive ? 'bg-surface-alt' : 'bg-surface'}`}

// Bad
className={cn('text-on-surface', isActive && 'bg-surface-alt')}
```

## Tailwind CSS 4 Patterns

### Semantic Tokens Only

Use theme tokens for all colors — never raw hex values in JSX:

```typescript
// Good
className="text-on-surface bg-surface-alt border-outline"

// Bad
className="text-[#1a1a1a] bg-[#f5f5f5] border-[#e0e0e0]"
```

### Rounded Corners

Use `rounded-[2px]` on all interactive elements (deliberate design choice).

### Responsive

Mobile-first: `md:` for tablet+, `lg:` for desktop side-by-side.

## Imports

### Organization

Group imports in order:

1. React / Next.js
2. External packages (motion, lenis, etc.)
3. Internal components and utilities
4. Types (with `type` keyword)
5. Assets / images

```typescript
import { useRef, useState } from 'react'
import Image from 'next/image'
import { motion, AnimatePresence } from 'motion/react'

import { ExternalLink } from '@/components/ui/ExternalLink'
import { siteConfig } from '@/app/site.config'

import type { NavItem } from '@/types'
```

### Direct Imports

No barrel `index.ts` files — use direct file path imports:

```typescript
// Good
import { ExternalLink } from '@/components/ui/ExternalLink'

// Bad
import { ExternalLink } from '@/components/ui'
```

## SVG and Images

- SVG icons: inline in JSX for UI icons
- Brand assets: `next/image` from `public/assets/`
- External links: always use the `ExternalLink` component

## Comments

### General Rules

- No emojis in comments
- Write in neutral, professional tone
- Comment **why**, not **what**
- No AI-generated context comments
- No commented-out code (delete it)

## Anti-Patterns

| Pattern | Problem | Solution |
|---------|---------|----------|
| `any` type | Loss of type safety | Use `unknown` + type guards |
| Raw hex colors | Breaks theme switching | Use semantic tokens |
| Hardcoded strings | Violates content system | Use `site.config.ts` |
| `cn()` / `clsx` | Unnecessary dependency | Plain template literals |
| Barrel exports | Bundle bloat | Direct file imports |
| `console.log` | Leaks to production | Remove or use dev flag |
| Default exports (non-page) | Hurts tree-shaking | Named exports |

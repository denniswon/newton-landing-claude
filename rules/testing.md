# Testing Guidelines

## Frameworks

- **Unit Tests**: Jest 30 + React Testing Library 16
- **E2E Tests**: Playwright 1.58 (Chromium)

## Test Organization

### Co-located Unit Tests

Place test files next to the source they test:

```
src/
├── components/
│   ├── sections/
│   │   ├── HeroSection.tsx
│   │   └── HeroSection.test.tsx
│   └── ui/
│       ├── NavButton.tsx
│       └── NavButton.test.tsx
├── app/
│   ├── site.config.ts
│   └── site.config.test.ts
```

### E2E Tests

Playwright tests live in a top-level directory:

```
e2e/
├── navigation.spec.ts
├── theme-toggle.spec.ts
└── whitepaper-form.spec.ts
```

## Unit Test Patterns

### Component Tests

```typescript
import { render, screen } from '@testing-library/react'
import { HeroSection } from './HeroSection'

describe('HeroSection', () => {
  it('renders the hero title from site config', () => {
    render(<HeroSection />)
    expect(screen.getByRole('heading', { level: 1 })).toBeInTheDocument()
  })

  it('renders CTA button with correct href', () => {
    render(<HeroSection />)
    const cta = screen.getByRole('link', { name: /get started/i })
    expect(cta).toHaveAttribute('href')
  })
})
```

### Config Integrity Tests

```typescript
import { siteConfig } from './site.config'

describe('site.config', () => {
  it('has all required section data', () => {
    expect(siteConfig.hero).toBeDefined()
    expect(siteConfig.hero.title).toBeTruthy()
    expect(siteConfig.hero.cta).toBeDefined()
  })

  it('all external links use https', () => {
    // Recursively check all href values
  })
})
```

## E2E Test Patterns

### Navigation

```typescript
import { test, expect } from '@playwright/test'

test('smooth scrolls to section on nav click', async ({ page }) => {
  await page.goto('/')
  await page.click('text=Features')
  await expect(page.locator('#features')).toBeInViewport()
})
```

### Theme Toggle

```typescript
test('toggles between light and dark theme', async ({ page }) => {
  await page.goto('/')
  const html = page.locator('html')
  const initialTheme = await html.getAttribute('data-theme')

  await page.click('[aria-label="Toggle theme"]')
  const newTheme = await html.getAttribute('data-theme')

  expect(newTheme).not.toBe(initialTheme)
})
```

## What to Test

- **Content rendering**: Sections render content from `site.config.ts`
- **Navigation**: Smooth scroll, mobile menu, anchor links
- **Theme**: Toggle, persistence across reload, no flash
- **External links**: `ExternalLink` component adds correct attributes
- **Responsive**: Layout changes at breakpoints (E2E)
- **Form submission**: Whitepaper email capture (E2E with mocked API)
- **Accessibility**: Keyboard navigation, ARIA attributes

## What NOT to Test

- Animation timing or spring physics (too brittle)
- Exact CSS values (test behavior, not style)
- Third-party library internals (Motion, Lenis)
- Static image rendering

## Before Committing

```bash
npx tsc --noEmit    # Type checking
npm run lint        # ESLint
npm test            # Jest unit tests
npm run build       # Verify build
```

Full validation: `npx tsc --noEmit && npm run lint && npm test && npm run build`

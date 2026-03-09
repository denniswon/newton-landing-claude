# Security Guidelines

## Landing Page Threat Model

This is a public marketing site. The threat model is narrower than an SDK or backend service, but still matters.

| Concern | Risk Level | Mitigation |
|---------|------------|------------|
| Secrets in client bundle | Critical | `LOOPS_API_KEY` is server-only, never `NEXT_PUBLIC_` prefixed |
| XSS via config or CMS | Medium | Content comes from `site.config.ts` (code-reviewed), not user input |
| Open redirect via external links | Medium | `ExternalLink` component with `rel="noopener noreferrer"` |
| Dependency supply chain | Medium | Lockfile pinning, minimal dependencies |
| Email injection in whitepaper form | Medium | Server-side validation in API route |

## Environment Variables

### Server-Only Secrets

`LOOPS_API_KEY` must only be used in `src/app/api/` route handlers. It must NEVER:
- Be prefixed with `NEXT_PUBLIC_`
- Be imported in any file outside `src/app/api/`
- Appear in client-side bundles
- Be logged or included in error messages

### Client-Side Variables

Only `NEXT_PUBLIC_SITE_URL` is safe for client use. Default to `https://newton.xyz` if not set.

## External Links

All external links must use the `ExternalLink` component, which adds:
- `target="_blank"` — opens in new tab
- `rel="noopener noreferrer"` — prevents `window.opener` access and referrer leaking

Never use raw `<a>` tags for external URLs.

## Form Security

The whitepaper download form (`/api/whitepaper/`) must:
- Validate email format server-side
- Rate limit submissions (Vercel handles basic rate limiting)
- Not reflect user input back in responses without sanitization
- Return generic error messages (no internal details)

## Content Security

- Content comes from `site.config.ts` — a TypeScript file in the codebase, not a CMS
- No `dangerouslySetInnerHTML` unless absolutely necessary and content is trusted
- No dynamic script injection

## Dependencies

Keep the dependency tree minimal for a marketing site:
- `next` — framework (required)
- `react` / `react-dom` — UI (required)
- `motion` — animations (small, well-maintained)
- `lenis` — smooth scroll (small, single-purpose)

Avoid adding dependencies for functionality that can be done with CSS or native browser APIs.

## Sensitive Data Handling

- No analytics data in localStorage beyond what GA4 manages
- No PII stored client-side
- Whitepaper email submissions sent directly to Loops.so — not stored locally

# PR Review Workflow

## Quick Start

```bash
# Review current branch changes
/review-pr

# Re-review after author addresses feedback
/re-review-pr https://github.com/newt-foundation/landing-page/pull/42
```

## What Gets Reviewed

Blockers — accessibility violations, leaked secrets, broken content system (`site.config.ts` bypass), raw hex colors instead of semantic tokens, missing `ExternalLink` usage, CLS-causing layout shifts, server-only secrets exposed client-side.

Concerns — missing responsive breakpoints, broken dark mode, missing image `alt` text, unnecessary `'use client'` directives, new dependencies without justification, missing tests for new components.

Minor notes — naming clarity, import organization, animation timing preferences.

## Comment Style Guidelines

### General Principles

- Be **direct and concise** - state the issue immediately without preamble
- No bold category prefixes like "**Critical:**" or "**Warning:**"
- No formal headers like "Suggested fix:" - just provide the fix
- No markdown headings (`##`, `###`) in the review body — use `**bold**` sparingly
- Reference existing patterns in the codebase when applicable
- Use conversational directives: "Let's not do this way" or "something like below can work"
- Use "ditto:" for repeated issues in different files
- Use "nit:" for minor/cosmetic issues
- For follow-up reviews: reference previous comments casually ("still not addressed from previous review")
- Keep the review body focused on what's wrong, not what's been fixed
- Don't comment on things that are correctly done
- Bold direct asks to the author: **Document this in the PR description.**
- Escalate clearly when blocking: "A blocker for this PR considering [reason]."

### Voice and Framing

- **Use "we/us" not "you"** — collaborative framing. "Let's do X", "if we update", "we should"
- **Direct imperatives for clear issues** — "DO NOT", "Shouldn't do this", "Let's do X"
- **Prefix labels for non-blocking items:**
  - `Opinion:` — subjective feedback, architectural preferences
  - `FYI:` — informational, good to know
  - `Suggestion:` — code improvement with example
  - `nit:` — minor/cosmetic
- **No praise sections** — review body is for issues only
- **No severity grouping** — flat numbered list with prefix labels
- **Short punchy comments preferred**
- **End with clear merge criteria** — "LGTM once above are addressed."

### Formatting Rules — DO NOT USE:

- Emoji headers or category prefixes
- Bold-label patterns like "**Problem:**", "**Risk:**", "**Fix:**"
- Markdown headings (`##`, `###`) in the review body
- Severity grouping headers
- Long code blocks in the review body (code suggestions go in inline comments)
- "What is working well" or praise sections
- Using "you" — use "we/us/let's"

### Inline Comment Format

Good:

```text
This hardcodes "Newton Protocol" directly. Let's pull it from site.config.ts — we have a content system for a reason.
```

```text
Raw hex `#1a1a1a` here. Let's use `text-on-surface` so it works in both themes.
```

### Code Suggestions

Only include code examples when the fix is non-obvious. Introduce casually: "something like below can work."

### Cross-References

Always use GitHub permalinks with commit SHA and line range:

- "We already do this in https://github.com/newt-foundation/landing-page/blob/abc123/src/components/ui/ExternalLink.tsx#L5-L12"

### Landing Page-Specific Concerns

When reviewing landing page code, additionally check:

- **Content integrity**: All strings from `site.config.ts`, no hardcoded text in components
- **Accessibility**: Semantic HTML, keyboard navigation, ARIA labels, color contrast in both themes
- **Performance**: Static vs dynamic import placement, `next/image` usage, `priority` only above fold
- **Responsive design**: Mobile-first, `md:` and `lg:` breakpoints, no horizontal overflow
- **Theme consistency**: Semantic tokens only, works in both light and dark mode
- **Visual consistency**: `rounded-[2px]` on interactive elements, consistent spacing
- **SEO**: Proper heading hierarchy, meaningful `alt` text, no content hidden from crawlers
- **External links**: All external hrefs use `ExternalLink` component

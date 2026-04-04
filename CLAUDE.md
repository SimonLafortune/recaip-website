# Recaip — recaip.com

AI-generated changelogs, release notes, and product updates from your dev tools. "Your users always know what you shipped, on autopilot."

## What is Recaip

Recaip connects to GitHub, Linear, Jira (and more) and automatically generates release notes every time you ship. It writes the recap and pushes it everywhere — changelog page, in-app announcement, email draft, social post, Slack, status page. Users review and approve, or set channels to auto-publish.

## Current State

**Phase: MVP build in progress.** Core loop works: connect GitHub repo → merge PR → AI generates changelog + social post → approve/reject → public changelog page. Dogfooding with its own repo (SimonLafortune/recaip). Not deployed yet (localhost only).

### Git workflow

**Always use branches + PRs** (not direct push to main) so Recaip can draft from merged PRs. GitHub CLI (`gh`) is installed and authenticated.

### Files

| File | Purpose |
|------|---------|
| `index.html` | Homepage — hero with sticky scroll-over effect, preloader, dark glassmorphism design. |
| `features.html` | Features page — output formats, how AI works, feature deep-dive. |
| `pricing.html` | Pricing page — single $19/mo plan, FAQ accordion, "what counts as a product" explainer. |
| `about.html` | About page — mission, founder, principles, tech stack. |
| `styles.css` | Shared CSS — glass cards, animations, ambient glows, nav, preloader styles. |
| `waitlist.html` | Archived waitlist version of the landing page. |
| `hero-bg.png` | Fullscreen hero background image (teal/green landscape illustration). |
| `screenshot-dashboard.png` | Product screenshot — dashboard with products and recent drafts. |
| `screenshot-product.png` | Product screenshot — product page with AI-generated drafts. |
| `screenshot-settings.png` | Product screenshot — settings with theme customization. |
| `logo.svg` | SVG wordmark — lowercase "recaip" in Inter Light (weight 300). |
| `webapp/` | **Next.js 16 app** — the actual product. See `webapp/CLAUDE.md` for full details. |

### What works now

- GitHub OAuth login via Supabase Auth
- Repo onboarding (pick repo → register webhook)
- AI draft generation from merged PRs (Claude Sonnet, v3 prompt with XML tags)
- Batch drafts — skips already-drafted PRs
- Dashboard: approve/reject drafts, copy social posts
- Public changelog at `/changelog/[slug]` (timeline design, customizable theme)
- Embeddable iframe at `/embed/[slug]`
- Product settings: accent color, font, light/dark mode

### What's NOT done (priority order)

1. Product knowledge onboarding — "Tell us about your product" step (biggest AI quality lever)
2. Deploy to Vercel — webhooks don't work on localhost
3. GitHub App migration — OAuth token expires, users have to re-login
4. Edit drafts before approving
5. Stripe billing
6. UI/UX polish pass (deliberately deferred — prototype-first approach)
7. Sign out button

### Known issues

- GitHub OAuth provider_token expires quickly — user has to clear cookies and re-login. Fix: migrate to GitHub App with installation tokens.
- Zombie node process on Windows — `taskkill /PID <pid> /F` doesn't always work from bash. Restart from Windows directly if stuck.

### Idea Documentation (in `Ideas/shiplog/`)

| File | Purpose |
|------|---------|
| `Ideas/shiplog/README.md` | Clean pitch doc — what Recaip is, ICP, pricing, scoring. |
| `Ideas/shiplog/PRD.md` | Full product requirements — personas, user stories, MVP scope, DB schema, pricing, GTM, kill criteria. |
| `Ideas/shiplog/NOTES.md` | Running discussion log — all research, competitive analysis, pricing evolution, decisions. |
| `Ideas/shiplog/TECHNICAL.md` | Technical implementation guide — architecture, webhooks, code samples, cost projections. |

> The folder is named `shiplog` for historical reasons (original name). The product is now **Recaip**.

## Design System

### Colors — Two backgrounds only

1. **Dark teal** (body default): `#0b1a18` — used for dark sections
2. **White** (`bg-white`) — used for light sections, alternating

Sections alternate between dark teal and white. No third background color.

### Brand Palette (teal, derived from hero image)

```
brand-50:  #e6f5f2   (lightest, bg tints on white sections)
brand-100: #ccebe5
brand-200: #99d7cb
brand-300: #6bbfae
brand-400: #4da895   (accents, icons on dark bg)
brand-500: #3d8b80   (primary buttons, key accents)
brand-600: #2d6b63   (button hover, strong accents)
brand-700: #1f524c
brand-800: #163d38
brand-900: #0e2a26   (darkest)
```

### Surface Palette (dark teal backgrounds)

```
surface-0: #0b1a18   (body bg)
surface-1: #0f2420   (card bg on dark sections)
surface-2: #142e2a   (input bg on dark sections)
surface-3: #1a3935
surface-4: #21453f   (borders on dark sections)
```

### Typography

- **Body:** Inter (400, 500, 600, 700, 800)
- **Code/mono:** JetBrains Mono (400, 500)
- **Hero title:** `font-semibold` (not extrabold — sleek, not heavy)
- **Logo:** Inter weight 300 (light), lowercase "recaip"

### Section Pattern

All-dark design. No white sections. Glass cards (`glass-card`, `glass-card-bright`, `glass-card-glow`) with `backdrop-filter: blur(16px)` and `rgba(255,255,255)` borders. Text uses `text-white/90` (headings), `text-white/50` (body), `text-white/40` (subtle). Ambient glow blobs behind key sections for depth.

### Buttons

- Primary: gradient `#3d8b80 → #2d6b63`, white text
- Glass: `bg-white/10 backdrop-blur`, white text, `border-white/15`

## Website Structure (multi-page)

### Homepage (index.html) sections:
1. **Preloader** — letter-by-letter "recaip" animation + tagline
2. **Hero** — sticky fullscreen `hero-bg.png`, content scrolls over it with rounded leading edge
3. **Integrations** — GitHub, Linear, Jira, Slack, GitLab, Notion + upcoming tool pills
4. **Pain Points** — 3 problem/solution cards (awareness gap, tedium tax, ownership vacuum)
5. **How it works** — 3 steps: Connect → Ship → Flip to autopilot. Walk away.
6. **Before/After** — webhook event → Recaip output side-by-side
7. **See it in action** — product screenshots + live changelog link (app.recaip.com/changelog/recaip-bb6480)
8. **Output formats** — bento grid: 2 hero cards + 4 smaller
9. **Features** — bento grid: 2 hero features + 6 smaller, accent bars
10. **Pricing** — single $19/mo plan, all features, first 10 recaps free
11. **Final CTA** — "Start free" + "See a live changelog"
12. **Footer** — logo, nav links (Features, Pricing, About), built by Simon

### Other pages:
- **features.html** — output formats deep dive, how AI works, feature breakdown
- **pricing.html** — single plan card, "what counts as a product" explainer, FAQ accordion
- **about.html** — mission, founder story, principles, tech stack

## Key Messaging

- **Title:** "Configure once. Never write a changelog again."
- **Subtitle:** "Recaip is an autonomous agent that listens to your dev tools 24/7. Every time you ship, it writes the update and publishes it. You never have to open Recaip again."
- **Tagline:** "Ship it. Recaip it."
- **Primary ICP:** CTO-founders at small startups (5-30 people) with no PM
- **Secondary ICP:** Solo vibe coders shipping fast with zero comms workflow
- **Tertiary ICP:** PMs at scaleups automating manual changelog writing
- **Core positioning:** Recaip is an always-on autonomous agent, not a tool you use. Configure once, it runs forever. Users never need to open Recaip again after setup.

## Pricing Model

Single plan. One price. Everything included. Per product (not per user, not per repo). A "product" = one thing you ship that users care about.

| | Details |
|---|---|
| Price | $19/product/mo |
| Trial | First 10 recaps free, full access, no credit card required |
| Includes | All 6 output formats, all integrations, auto-publish, Slack delivery, weekly digest, tone learning, custom domain, status page integrations, no branding, unlimited repos per product |

No free tier. No feature gates. No tiers to compare.

## Planned Tech Stack (for MVP build)

- **Frontend:** Next.js 15 (App Router) + Tailwind CSS + Vercel
- **Backend:** Supabase (auth via GitHub OAuth, Postgres DB, edge functions)
- **AI:** Claude API via AI SDK (Sonnet for drafts, Haiku for classification)
- **Payments:** Stripe
- **Integrations:** GitHub API (webhooks), Linear API, Jira API, Slack API
- **Build tools:** Claude Code (backend/logic) + Lovable (UI rapid prototyping)

## Waitlist Infrastructure

The landing page form POSTs to Supabase REST API. Replace these placeholders in `index.html`:
- `YOUR_SUPABASE_URL`
- `YOUR_SUPABASE_ANON_KEY`

Supabase table needed:
```sql
create table waitlist (
  id uuid default gen_random_uuid() primary key,
  email text unique not null,
  utm_source text default 'direct',
  created_at timestamptz default now()
);
```

## Conventions

- Keep the landing page as a single `index.html` — no build step, no framework, just static HTML + Tailwind CDN
- When the MVP app is built, it will be a separate Next.js project (potentially in this same folder or a subfolder)
- All idea documentation lives in `Ideas/shiplog/` — update NOTES.md for discussion, README.md for the pitch, PRD.md for requirements
- Hero image must not be replaced without Simon's approval — it sets the brand tone

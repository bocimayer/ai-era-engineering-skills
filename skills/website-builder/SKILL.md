---
name: website-builder
description: Decision guide for what stack, tools, and structure to use whenever the user asks Claude to build, create, or scaffold a website, landing page, or web app. Use before writing any code for a new website project.
---

# Website Builder Guide

Before writing a single file, figure out what kind of site this actually is.
Most wasted effort on website requests comes from reaching for a framework a
brochure page didn't need, or hand-rolling HTML for something that will grow
into an app. Pick the smallest stack that fits the request, and say what you
picked and why in one sentence before you start.

## Step 1 — Classify the request

Ask yourself (don't necessarily ask the user, unless the answer changes the
stack materially):

- **How many pages/screens?** One vs. several vs. "grows over time."
- **Does it need interactivity or client state?** Forms that just submit
  don't count; a filterable table, auth, or a dashboard does.
- **Does it need a backend or database?** User accounts, saved data,
  payments, anything beyond a static contact form.
- **Who maintains it after this session?** A non-technical owner favors
  plain files or a hosted builder-style stack over a framework with a build
  step they can't run.
- **Where will it be hosted?** If the user already has a host/platform in
  mind (Vercel, Netlify, GitHub Pages, existing server), let that constrain
  the stack rather than picking one that fights it.

If the user's request already names a stack, a language, or an existing
codebase to extend, use that — this guide is for filling the gap when they
didn't specify one, not for overriding an explicit choice.

## Step 2 — Check for a design source

If the user references a Figma file (a link, "match this Figma," an existing
design system), don't eyeball a screenshot. Use the Figma MCP connector if
it's connected (`get_design_context`, `get_screenshot`, `get_variable_defs`,
`create_design_system_rules`) to pull the real spacing scale, color tokens,
type scale, and component structure, and build against those values instead
of guessing pixel values from an image. If the connector isn't connected,
tell the user it would make the handoff more accurate and ask if they want
to connect it (claude.ai connector settings) before proceeding with a
screenshot-only approximation.

No design source means no visual spec to match — proceed on taste (Step 5)
and the user's copy/content instead of inventing a brand.

## Step 3 — Pick the stack

| Situation | Default stack |
|---|---|
| Single static page (landing page, portfolio, resume, brochure) | Plain HTML + CSS + a little vanilla JS. No build step, no framework. Ships as one or a few files. |
| Multi-page content site (marketing site, docs, blog) with no/light interactivity | [Astro](https://astro.build) — ships zero JS by default, supports Markdown content collections, scales to many pages without a SPA's overhead. |
| Interactive web app (auth, dashboards, forms with client state, anything SPA-shaped) | Next.js (App Router) + TypeScript + Tailwind CSS. This is the safe modern default — huge ecosystem, works on Vercel with zero config. |
| Needs a backend/database | Extend the Next.js app with a hosted Postgres (Supabase or Neon) and an ORM (Drizzle or Prisma) rather than hand-rolling a separate API server, unless the user already has a backend. |
| User explicitly wants "no build tools" / static hosting only (e.g. GitHub Pages with no Actions) | Plain HTML/CSS/JS even for a few pages — don't introduce a framework they can't deploy. |

Do not default to React/Next.js for a one-page static site — that's the
single most common over-engineering mistake here. Do not hand-roll vanilla
JS for something that clearly needs client-side routing and state — that's
the opposite mistake.

## Step 4 — Fill in the rest of the stack

- **Styling:** Tailwind CSS by default for anything with a build step;
  plain CSS (with custom properties for theme values) for static HTML sites.
  Don't add a component library (shadcn/ui, MUI, etc.) unless the app has
  enough interactive surface area to justify it — a landing page doesn't.
- **Common UI primitives (toasts, dialogs, command menus, OTP inputs,
  charts, drag-and-drop):** don't hand-roll these. If the
  [`pick-ui-library`](https://github.com/emilkowalski/skill) skill is
  installed, invoke it to get the curated pick for the specific task
  (e.g. Sonner for toasts, base-ui for unstyled accessible primitives,
  motion for spring/gesture animation). Otherwise check the project's
  `package.json` for an existing choice before adding a new dependency.
- **TypeScript:** default on for anything framework-based; skip it for
  plain static sites unless the user asks.
- **Images/assets:** use the framework's built-in image optimization
  (`next/image` for Next.js, Astro's `<Image />`) rather than raw `<img>`
  tags when one is available.
- **Forms:** a static contact form posts to a form backend (Formspree,
  Netlify Forms, or a simple serverless function) rather than requiring a
  full backend just to receive email.
- **Deployment:** Vercel for Next.js/Astro unless the user names a
  different target; static HTML deploys anywhere (GitHub Pages, Netlify, S3)
  — don't assume Vercel is required for it.

## Step 5 — Taste and polish

Generic scaffolds read as generic because the interactive states, motion,
and layout defaults were never questioned, not because the color palette was
wrong. Two dedicated, installable skills cover this better than a bullet
list can — check whether either is installed and load it before writing UI
code:

- **[`emilkowalski/skill`](https://github.com/emilkowalski/skill)**
  (`npx skills@latest add emilkowalski/skills`) — animation and
  component-craft taste from Emil Kowalski (ex-Vercel/Linear, author of
  Sonner and Vaul). Use **`emil-design-eng`** for general UI polish
  (easing choice, transform-origins, hover/press feedback), **`animate`**
  when building a specific animation from scratch, **`review-animations`**
  to audit motion already in the codebase, and **`pick-ui-library`** for
  the component-library decision in Step 4.
- **[`leonxlnx/taste-skill`](https://github.com/leonxlnx/taste-skill)**
  (`npx skills add https://github.com/Leonxlnx/taste-skill`) — anti-slop
  layout/visual-direction taste for landing pages, portfolios, and
  redesigns. It reads the brief, infers a design direction, and tunes
  three dials (layout variance, motion intensity, visual density) instead
  of defaulting to the same generic template. Use its default
  `design-taste-frontend` skill for greenfield builds and
  `redesign-existing-projects` when improving an existing UI.

If neither is installed, apply this fallback checklist instead:

- Give every interactive element real hover, focus, active, and disabled
  states — not just the default browser outline. Focus states must stay
  visible for keyboard users; never remove `outline` without replacing it.
- Use purposeful transitions (150–250ms, an eased curve like
  `ease-out`/`cubic-bezier`, not linear) on state changes — hover, open/close,
  toggle. Motion should clarify what changed, not decorate. Respect
  `prefers-reduced-motion` by disabling non-essential motion for users who
  request it.
- Design the empty, loading, and error states, not just the happy path —
  these are where unpolished sites show it fastest.
- Avoid the default "AI-generated" look: purple/blue gradient hero, every
  corner at the same `rounded-2xl`, generic drop shadows on everything, stock
  hero illustration. Ground color, radius, and spacing choices in the
  specific brand/content instead of a generic default.
- Keep spacing and sizing on a consistent scale (e.g. Tailwind's default
  scale) rather than one-off pixel values scattered through the code.

Apply whichever version fires whatever stack Step 3 picked, including plain
HTML/CSS — none of this is React-specific.

## Step 6 — Baseline quality bar

Regardless of stack, before calling a website "done":

- Responsive at mobile widths (~375px) through desktop — check it, don't
  assume it. If Playwright is available in the environment, launch the site
  and screenshot it at a mobile and a desktop viewport, and click through the
  primary flow (nav, the main form, any interactive component) rather than
  reading the code and assuming it works — the `run` skill covers launching
  and driving the app.
- Semantic HTML (`<nav>`, `<main>`, `<header>`, `<button>` not `<div
  onclick>`) and real alt text on images — this is both accessibility and
  SEO.
- A `<title>` and meta description on every page.
- No console errors, no broken links, no placeholder Lorem Ipsum left in
  content the user actually gave you real copy for.
- Light/dark rendering only if the user asked for a theme toggle — don't
  add one speculatively.
- Anything with forms, auth, or user data gets a pass through the
  `security-review` skill before shipping.

## Step 7 — Say what you picked

State the chosen stack and the one-line reason in your first response
before generating files, e.g. "This is a five-page marketing site with no
interactivity, so I'll build it as static HTML/CSS — no framework needed."
This gives the user a cheap chance to redirect before you've written code
around the wrong assumption.

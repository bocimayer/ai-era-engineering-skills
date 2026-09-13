---
name: website-builder
description: Decision guide for what stack, tools, and structure to use whenever the user asks Claude to build, create, or scaffold a website, landing page, or web app. Use before writing any code for a new website project.
---

# Website Builder Guide

Before writing a single file, figure out what kind of site this actually is.
Most wasted effort on website requests comes from reaching for a framework a
brochure page didn't need, or hand-rolling HTML for something that will grow
into an app. Pick the smallest stack that fits the request, decide the
visual direction once up front, and say both before you start.

The steps below aren't independent options to mix and match — they're an
order of operations. Classification gates which taste tool even applies;
the design direction has to be decided before component code exists, not
patched on after; and the audit/verification tools only make sense once
there's something built to audit.

## Step 1 — Classify the request

Ask yourself (don't necessarily ask the user, unless the answer changes the
stack materially):

- **Greenfield or existing?** Building new, or modifying/redesigning a site
  that already exists? This decides which taste tool applies later (Step 4)
  — a fresh build and a redesign use different ones, and use them at
  different points (before writing code vs. auditing what's there).
- **What kind of UI?** Landing page, portfolio, or marketing site vs. a
  dashboard, data table, or multi-step product app. This also gates Step 4:
  some taste tools explicitly scope themselves to the first category and
  refuse the second.
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

A design source changes what Step 4 needs to decide: with real tokens in
hand, the visual direction (color, type, spacing, layout) is already given
— Step 4's job shrinks to filling the gaps Figma doesn't specify (motion,
interactive states) rather than inventing a direction from scratch. With no
design source, Step 4 has to do that inference itself.

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

## Step 4 — Decide the visual direction

This has to happen before any component gets written — it's not a polish
pass you do at the end. Which tool applies depends on what Step 1 and
Step 2 established:

- **Greenfield landing page, portfolio, or marketing site, no design
  source:** if
  [`leonxlnx/taste-skill`](https://github.com/leonxlnx/taste-skill)
  (`npx skills add https://github.com/Leonxlnx/taste-skill`) is installed,
  invoke its `design-taste-frontend` skill. It reads the brief, infers page
  kind/audience/vibe, and sets three dials (layout variance, motion
  intensity, visual density) that gate every layout and motion decision
  that follows — this is what produces a one-line "Design Read" instead of
  defaulting to the same generic template.
- **Existing site being redesigned:** use the same repo's
  `redesign-existing-projects` skill instead — it audits the current UI
  and flags generic-AI patterns before changing anything, rather than
  inferring a direction from a blank brief.
- **Dashboard, data-dense, or multi-step product UI:** `design-taste-frontend`
  explicitly excludes this category — don't invoke it here. Fall back to
  conventional patterns (shadcn/ui or base-ui + Tailwind) and rely on
  general craft philosophy instead (see Step 6's `emil-design-eng`).
- **A design source already exists (Step 2):** the direction is already
  decided by the Figma tokens. Skip the brief-inference step above and go
  straight to filling gaps Figma doesn't cover — motion and interactive
  states — using the tools in Step 6 while building.

## Step 5 — Say what you picked

Before generating any files, state the stack (Step 3) and the design
direction (Step 4) together, in one or two sentences, e.g. "This is a
five-page marketing site with no interactivity and no existing design, so
I'll build it as static HTML/CSS, reading the brief as a calm, editorial
portfolio direction." This gives the user a cheap chance to redirect before
you've written code around the wrong assumption on either axis.

## Step 6 — Build it

Fill in the rest of the stack, and reach for these tools as the specific
need comes up during implementation — not as a single upfront checklist:

- **Styling:** Tailwind CSS by default for anything with a build step;
  plain CSS (with custom properties for theme values) for static HTML sites.
  Don't add a component library (shadcn/ui, MUI, etc.) unless the app has
  enough interactive surface area to justify it — a landing page doesn't.
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
- **Hit a common UI primitive** (toast, dialog, command menu, OTP input,
  chart, drag-and-drop)? If
  [`pick-ui-library`](https://github.com/emilkowalski/skill) is installed,
  invoke it explicitly — it doesn't trigger on its own — for the curated
  pick (Sonner for toasts, base-ui for unstyled accessible primitives,
  motion for spring/gesture animation). Otherwise check the project's
  `package.json` for an existing choice before adding a new dependency.
- **Writing an interactive element** (button, popover, hover card, toggle)?
  Consult `emil-design-eng`'s craft rules as you write it — correct easing
  direction, transform-origin anchored to the trigger, real active/press
  feedback — rather than shipping the default browser/CSS-framework state
  and polishing later.
- **Building a specific animation from scratch?** Use `animate` — it makes
  the should-it-animate/curve/duration/exit decisions in the order that
  determines whether it feels right, and writes the implementation.
- **Unsure whether something should animate at all?** `find-animation-opportunities`
  is read-only — it proposes exact values without implementing, useful when
  scoping motion before deciding what to build.

## Step 7 — Pre-ship audit

Once the build is functionally complete, audit and verify before calling it
done — these are review/verification tools, not implementation tools:

- If `review-animations` is installed, invoke it explicitly against the
  motion you just wrote — it defaults to flagging, approval is earned. For
  a large existing codebase rather than a fresh build, `improve-animations`
  produces a prioritized audit and plan instead (read-only; it doesn't
  apply fixes itself).
- Responsive at mobile widths (~375px) through desktop — check it, don't
  assume it. If Playwright is available in the environment, launch the site
  and screenshot it at a mobile and a desktop viewport, and click through
  the primary flow (nav, the main form, any interactive component) rather
  than reading the code and assuming it works — the `run` skill covers
  launching and driving the app.
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

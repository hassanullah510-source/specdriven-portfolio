# Feature Specification — Personal Portfolio

**Feature:** `001-portfolio`
**Status:** Draft — approved content, ready to implement
**Version:** 1.0
**Created:** 2026-10-25
**Owner:** Hassan Ullah
**Input:** "I need to make a portfolio by using spec-driven development."

---

## 1. Overview

A single-page personal portfolio for **Hassan Ullah**, a Full Stack & JavaScript
developer. The site presents his positioning, selected work, skills, experience,
services, and contact details in a bold editorial design.

It is a static website — semantic HTML, modern CSS, and vanilla JavaScript — deployed
to GitHub Pages and maintained by editing a single HTML file. There is no framework,
no build step, and no runtime dependency.

**Design direction:** SIGNAL — editorial neo-brutalist dark theme, acid-lime and magenta
accents, oversized type, numbered sections, grain texture, and optional motion.

## 2. Goals

- Communicate who Hassan is and what he does within 10 seconds of landing.
- Make six real projects easy to scan and easy to link to.
- Show a credible skills and experience narrative without unverifiable claims.
- Provide a frictionless contact path (email) with no backend.
- Score Lighthouse ≥ 95 in every category on mobile.
- Be fully usable with a keyboard, a screen reader, reduced motion, and JavaScript off.
- Give the owner a simple, documented way to update content and links.
- Serve as a showcase of accessible, performant, hand-written front-end craft.

## 3. Non-goals

- No blog, CMS, authentication, database, or server-side code.
- No analytics, tracking pixels, or third-party scripts.
- No contact-form backend or email delivery service.
- No photo, résumé download, or case-study sub-pages in v1.
- No theme toggle, multi-language support, or client-side rendering of primary content.
- No fabricated clients, testimonials, metrics, employers, dates, or project links.

## 4. Personas

| Persona | Need |
| --- | --- |
| Recruiter / hiring manager | Quickly judge role, seniority, stack, and contact path. |
| Potential client | Understand services and see relevant project work. |
| Fellow developer | Inspect project links and front-end code quality. |
| Owner (Hassan) | Update copy, projects, and links without a build step. |

## 5. User stories

Priority: **P1** = required for v1, **P2** = valuable, can slip without breaking the
release.

### US1 — First impression (P1)

As a recruiter, I want to see the developer's name, role, and availability immediately,
so that I know whether to keep reading.

**Acceptance criteria**

1. The hero contains the name, "Full stack & JavaScript developer", a one-line value
   statement, and a primary call to action above the fold at 360 × 640.
2. The hero has a secondary "Email me" action that opens the user's mail client.
3. The hero shows an "Open to work" status that does not rely on colour alone.
4. No layout shift occurs while fonts load.
5. The hero remains readable and complete with JavaScript disabled.

### US2 — Evaluate work (P1)

As a hiring manager, I want to scan projects with their stack and outcome, so that I can
judge fit.

**Acceptance criteria**

1. Six projects are listed, each with an index, title, one-sentence description, and
   technology tags.
2. Each project shows a "Link pending" label instead of a dead link until a real URL is
   supplied.
3. When a URL exists, the project exposes an accessible link with an external-link
   indication and `rel="noopener"`.
4. Project rows are readable at 320 px and do not rely on hover to reveal information.
5. A live/repository link is only shown if it resolves to a real page.

### US3 — Assess skills and experience (P1)

As a recruiter, I want grouped skills and a credible experience narrative, so that I can
match the developer to a role.

**Acceptance criteria**

1. Skills are grouped into Frontend, Backend, and Tooling.
2. No arbitrary percentage bars or unverifiable proficiency scores are shown.
3. The experience section states "6+ years" once and describes four capability phases
   without inventing employers or dates.
4. Each group has a visible heading and uses list semantics.

### US4 — Understand services (P2)

As a potential client, I want to know what work is offered, so that I can decide to
reach out.

**Acceptance criteria**

1. Seven services are presented as an accordion.
2. The accordion works with keyboard only using native `<details>`/`<summary>`.
3. All content is available without JavaScript.
4. The first service is expanded by default; the others are collapsed.

### US5 — Contact (P1)

As a visitor, I want to contact Hassan with one click, so that I can start a
conversation.

**Acceptance criteria**

1. The contact section shows the email address as a `mailto:` link.
2. A copy button copies the address and announces success via a live region.
3. If clipboard access fails, the button reports failure and the mailto link still works.
4. GitHub and email links are labelled and open safely (`rel="noopener"` on external
   links).
5. No contact form, CAPTCHA, or third-party form service is used.

### US6 — Mobile and keyboard use (P1)

As a mobile or keyboard-only visitor, I want to navigate without a mouse.

**Acceptance criteria**

1. A skip link moves focus to the main content.
2. The mobile menu opens as a modal dialog, traps focus, closes on Escape, and returns
   focus to the trigger.
3. Navigation links move focus to the corresponding section heading.
4. The current section is indicated in the navigation without relying on colour alone.
5. Tap targets are at least 44 × 44 px.
6. There is no horizontal scrolling from 320 px to 1920 px.

### US7 — Motion sensitivity (P2)

As a visitor who prefers reduced motion, I want animations disabled.

**Acceptance criteria**

1. With `prefers-reduced-motion: reduce`, marquees, reveals, and pointer effects are
   disabled and content is immediately visible.
2. No animation runs longer than 5 seconds or flashes more than three times per second.
3. Scroll behaviour becomes immediate rather than smooth.

### US8 — Owner maintenance (P2)

As the owner, I want a documented editing workflow.

**Acceptance criteria**

1. The README explains how to edit projects, links, copy, and colours.
2. All outstanding placeholders are listed in the README.
3. Local preview works with any static server, and direct file open shows all content
   (JS enhancements may differ on `file://`).
4. Adding a project means copying one documented `work-item` block.
5. Deployment through GitHub Actions is documented step by step.

### US9 — Shareable and discoverable (P2)

As someone sharing the link, I want a rich preview and correct metadata.

**Acceptance criteria**

1. The page has a descriptive title, meta description, canonical URL, and Open
   Graph/Twitter tags with a 1200 × 630 social image.
2. JSON-LD `Person` data matches the visible content.
3. `robots.txt` and `sitemap.xml` reference the canonical URL.
4. A custom 404 page links back to the home page.
5. The favicon and web manifest are present and valid.

---

## 6. Functional requirements

| ID | Requirement |
| --- | --- |
| FR-001 | Serve a single page at `index.html` with sections: hero (`#top`), work (`#work`), about (`#about`), skills (`#skills`), experience (`#experience`), services (`#services`), contact (`#contact`). |
| FR-002 | Persist navigation to all sections in a fixed header with a scroll progress indicator. |
| FR-003 | Provide a full-screen mobile navigation dialog below 760 px. |
| FR-004 | Render six project rows with index, title, description, tags, and link state. |
| FR-005 | Mark missing project URLs with a visible "Link pending" state and no clickable dead link. |
| FR-006 | Group skills into three categories with list semantics and no percentage values. |
| FR-007 | Present four experience phases, the "6+ years" statement, and no invented employers or dates. |
| FR-008 | Present seven services in a native, accessible accordion. |
| FR-009 | Provide `mailto:` contact, copy-to-clipboard, and a status live region. |
| FR-010 | Link to GitHub (`hassanullah510-source`) and email; omit unknown social profiles. |
| FR-011 | Provide a custom 404 page that links back to the home page. |
| FR-012 | Provide favicon, web manifest, Open Graph/Twitter metadata, JSON-LD, `robots.txt`, and `sitemap.xml`. |
| FR-013 | Use only relative URLs for internal navigation and asset references. |
| FR-014 | Provide a footer with quick links, copyright, and a back-to-top control. |
| FR-015 | Provide `prefers-reduced-motion` and no-JS fallbacks for all enhancements. |
| FR-016 | Self-host both typefaces (Space Grotesk, JetBrains Mono) with `font-display: swap` and preload the Latin subsets. |
| FR-017 | Expose a visible focus indicator on every interactive element. |
| FR-018 | Announce dynamic status changes (copied, menu state) through a polite live region. |

## 7. Non-functional requirements

| ID | Requirement |
| --- | --- |
| NFR-001 | WCAG 2.2 AA; Lighthouse Accessibility 100. |
| NFR-002 | Performance budgets in the constitution are met. |
| NFR-003 | Responsive from 320 px to 1920 px without horizontal scrolling. |
| NFR-004 | Works in the latest two versions of Chrome, Edge, Firefox, and Safari. |
| NFR-005 | Semantic HTML passes the Nu HTML Checker with zero errors. |
| NFR-006 | All content and links work with JavaScript disabled. |
| NFR-007 | No third-party network requests at runtime. |
| NFR-008 | Page weight (HTML + CSS + JS + fonts) ≤ 220 KB, excluding the social image. |
| NFR-009 | 200% zoom and 320 px reflow preserve all content and actions. |
| NFR-010 | No console errors or warnings in production. |
| NFR-011 | Contrast: 4.5:1 body text, 3:1 large text and UI components. |
| NFR-012 | Motion is fully disabled under `prefers-reduced-motion: reduce`. |

## 8. Performance budgets

| Resource | Budget |
| --- | --- |
| HTML (`index.html`) | ≤ 45 KB uncompressed |
| CSS | ≤ 32 KB uncompressed |
| JavaScript | ≤ 18 KB uncompressed |
| Fonts | ≤ 100 KB total |
| Images | ≤ 300 KB total; lazy-load below the fold |
| Lighthouse Performance | ≥ 95 |
| Lighthouse Best Practices | ≥ 95 |
| Lighthouse SEO | 100 |
| Cumulative Layout Shift | < 0.05 |
| LCP on throttled 4G | < 2.0 s |

## 9. Content inventory

| Field | Value |
| --- | --- |
| Name | Hassan Ullah |
| Role | Full Stack & JavaScript Developer |
| Email | `hassanullah510@gmail.com` |
| GitHub | `https://github.com/hassanullah510-source` |
| Location | Not claimed |
| Availability | Open to work |
| Experience | 6+ years (owner-supplied) |
| Résumé / photo | Not included in v1 |

**Projects — all owner-confirmed real; live/repo URLs pending:**

| # | Project | Stack |
| --- | --- | --- |
| 01 | E-Commerce Website | React, Node.js, MongoDB, Stripe |
| 02 | Admin Dashboard | Next.js, TypeScript, SQL, Charts |
| 03 | Full Stack Banking System | React, Express, SQL, JWT |
| 04 | Task Management App | React, Node.js, MongoDB |
| 05 | Portfolio / CMS | Next.js, MongoDB, REST API |
| 06 | Real-Time Chat Application | React, Socket.io, Node.js |

**Skills**

- **Frontend:** HTML5, CSS3, JavaScript, TypeScript, React.js, Next.js, Responsive Web
  Design, UI/UX
- **Backend:** Node.js, Express.js, REST APIs, MongoDB, SQL
- **Tooling:** Git & GitHub, Full Stack Development

> Final grouping in the build must match the three `<ul>` groups in the skills section;
> adjust this list if the implementation groups items differently.

**Services:** Full Stack Web Development, Frontend Development, Backend Development,
JavaScript Development, API Development, Database Development, Website Optimization.

## 10. Assumptions

- The GitHub username `hassanullah510-source` is correct (from the owner's Git config).
- The six projects are real and belong to the owner; only the URLs are missing.
- "6+ years" is accurate and approved by the owner.
- The repository will be named `hassan-portfolio`, producing the Pages URL
  `https://hassanullah510-source.github.io/hassan-portfolio/`.
- X/Twitter and LinkedIn are intentionally omitted until verified.
- Content is edited directly in `site/index.html`; no CMS or data source is required.

## 11. Out of scope

Blog, case-study pages, light/dark theme toggle, multi-language support, contact-form
backend, analytics, résumé download, profile photo, testimonials, client logos, and any
content requiring a CMS.

## 12. Dependencies

- GitHub account `hassanullah510-source` and a repository for Pages deployment.
- GitHub Actions enabled for the repository (Pages build and deployment source set to
  **GitHub Actions**).
- Owner-supplied project URLs, and optionally LinkedIn/X profiles, before those links
  can be published.
- Local Node (`npx`) or Python for preview and validation only — not required to serve
  the site.

## 13. Risks

| Risk | Impact | Mitigation |
| --- | --- | --- |
| Project URLs never supplied | Six projects remain unlinked | Visible "Link pending" labels; tracked in README open items. |
| Repo name differs from assumption | Broken canonical/OG/sitemap URLs | Documented single place to update; validated in checklist. |
| Fonts exceed budget | Slower LCP | Latin subset, WOFF2, preload, `font-display: swap`; measured. |
| Decorative motion harms accessibility | Fails WCAG | Reduced-motion parity and manual walkthrough before release. |
| Content drift across sections | Inconsistent claims | Content inventory above is the single source of truth. |

## 14. Success criteria

1. A first-time visitor can identify the role, view all six projects, and contact Hassan
   in under 60 seconds.
2. Lighthouse mobile: Performance ≥ 95, Accessibility 100, Best Practices ≥ 95, SEO 100.
3. Zero axe-core critical or serious violations.
4. All links resolve or are explicitly marked pending.
5. The owner can add a project by editing one documented block.
6. The site passes the manual keyboard, reduced-motion, and no-JS walkthroughs.
7. HTML validates with zero errors.

## 15. Open items

| ID | Item | Handling |
| --- | --- | --- |
| OI-1 | Live/repo URL for each of the six projects | "Link pending" label + README checklist. |
| OI-2 | Confirm repository name and Pages URL | README documents every place to update. |
| OI-3 | LinkedIn / X profiles | Omitted until supplied. |
| OI-4 | Photo and résumé | Out of scope for v1. |

## 16. Approval

| Role | Name | Date |
| --- | --- | --- |
| Owner / product | Hassan Ullah | 2026-10-25 |
| Implementation | Pending | — |

**Next artefact:** `plan.md` — architecture, design system, validation strategy, and
constitution check.

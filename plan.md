# Implementation Plan — Personal Portfolio

**Feature:** `001-portfolio`
**Status:** Draft — ready to implement
**Version:** 1.0
**Created:** 2026-10-25
**Spec:** [spec.md](./spec.md)
**Constitution:** [constitution.md](../../.specify/memory/constitution.md)

---

## 1. Technical context

| Concern | Decision |
| --- | --- |
| Language | HTML5, CSS3 (native cascade layers), JavaScript ES2020 |
| Frameworks | None |
| Build step | None |
| Runtime dependencies | None |
| Dev-only tooling | `npx` for `serve`, `lighthouse`, and `vnu-jar` |
| Fonts | Self-hosted Space Grotesk (variable) + JetBrains Mono, Latin subset, WOFF2 |
| Hosting | GitHub Pages, published from `site/` via GitHub Actions |
| Preview | `npx.cmd serve site` or `python -m http.server 8080 --directory site` |
| Testing | Lighthouse CLI, Nu HTML Checker, axe DevTools, manual keyboard/motion/no-JS checks |
| Target browsers | Latest two versions of Chrome, Edge, Firefox, Safari |

**Rationale:** the owner needs a portfolio that loads fast, is cheap to host, and can be
updated by editing one file. A static page with hand-written CSS/JS meets every
requirement with the least machinery.

## 2. Constitution check

Compliance is verified twice: after planning (below) and before release. The
constitution lives at `.specify/memory/constitution.md` and is the highest authority;
if it is not present in this workspace, add it before starting implementation.

| Principle | Status | How this plan satisfies it |
| --- | --- | --- |
| I. Honest content | Pass by design | Only owner-confirmed facts; `Link pending` labels; open items tracked in README. |
| II. Static first | Pass by design | No framework/build; relative paths; HTML is the content source. |
| III. Accessible | Pass by design | Landmarks, skip link, dialog focus management, native accordion, reduced motion, AA contrast. |
| IV. Performance | Pass by design | Self-hosted subset fonts, deferred JS, CSS/SVG visuals, explicit budgets. |
| V. Design system | Pass by design | All values as tokens in `@layer tokens`; shared components. |
| VI. Maintainability | Pass by design | One HTML file; one documented project block; progressive enhancement. |
| VII. Privacy | Pass by design | `mailto:` + clipboard only; no analytics or third parties. |

No deviations or complexity exceptions are planned. Any later deviation must be recorded
in section 12.

## 3. Project structure

```text
.
├─ .specify/memory/constitution.md    # principles and budgets
├─ specs/001-portfolio/
│  ├─ spec.md                         # what and why
│  ├─ plan.md                         # this file
│  ├─ tasks.md                        # ordered work
│  └─ checklist.md                    # release checks
├─ site/                              # deployable document root
│  ├─ index.html                      # all primary content
│  ├─ 404.html
│  ├─ robots.txt
│  ├─ sitemap.xml
│  ├─ site.webmanifest
│  └─ assets/
│     ├─ css/styles.css               # layered design system + components
│     ├─ js/main.js                   # progressive enhancement only
│     ├─ fonts/                       # 2 × WOFF2 + OFL licence
│     └─ img/                         # favicons, touch icon, social preview
├─ tools/
│  ├─ og-template.html                # social image source
│  └─ icon.html                        # favicon render source
├─ .github/workflows/pages.yml        # GitHub Pages deployment
├─ .gitignore
├─ AGENT.md
└─ README.md
```

`site/` is the publish root. Nothing outside it is deployed.

## 4. Architecture

### 4.1 HTML document

- One document with labelled landmarks: `banner`, `nav`, `main`, `footer`.
- Reading order: hero (`#top`) → work (`#work`) → about (`#about`) → skills
  (`#skills`) → experience (`#experience`) → services (`#services`) → contact
  (`#contact`).
- One `h1` (the name). Each section has an `h2`; project titles and service summaries
  are `h3`/`summary` at the correct depth.
- Primary content is static; JavaScript never injects copy, projects, or links.
- Every section is linked from the primary nav, mobile nav, and footer quick links.
- Scroll-spy targets the section ids above; nav labels match the visible section names.

**Project block pattern** — copy one block, change the fields:

```html
<li class="work-item" style="--accent: var(--acid)">
  <span class="work-index" aria-hidden="true">01</span>
  <div class="work-body">
    <h3 class="work-title" id="project-1-title">E-Commerce Website</h3>
    <p class="work-desc">One sentence on the problem, the build, and the outcome.</p>
    <ul class="work-tags" aria-label="Technologies">
      <li>React</li><li>Node.js</li><li>MongoDB</li><li>Stripe</li>
    </ul>
  </div>
  <div class="work-meta">
    <span class="pending">Link pending</span>
    <!-- replace with the anchor below once a real URL exists -->
  </div>
</li>
```

Publishing a project replaces the pending span with:

```html
<a class="btn btn-ghost" href="https://example.com" target="_blank" rel="noopener">
  Visit <span aria-hidden="true">↗</span>
  <span class="sr-only">(opens in a new tab)</span>
</a>
```

Never emit an anchor with `href="#"`, an empty `href`, or a placeholder URL.

### 4.2 CSS

One stylesheet, ordered by native cascade layers:

```css
@layer reset, tokens, base, layout, components, utilities;
```

| Layer | Contents |
| --- | --- |
| `reset` | Box sizing, media defaults, reduced-motion reset, `prefers-reduced-motion` guard |
| `tokens` | Colour, type scale, spacing, radius, shadow, easing, container widths |
| `base` | Element styles, typography, selection, focus-visible ring |
| `layout` | Header, section shells, grids, footer, container queries where useful |
| `components` | Buttons, work rows, chips/tags, accordion, marquee, dialog, toast, stats |
| `utilities` | `.sr-only`, skip link, reveal states, back-to-top |

Rules:

- Tokens are declared once; no hard-coded colours or spacing in component rules.
- Prefer logical properties (`margin-inline`, `padding-block`) and intrinsic sizing
  (`clamp()`, `minmax()`, `auto-fit`).
- Fluid type via `clamp()`; no fixed pixel type on body copy.
- Breakpoints: ~760 px for the desktop nav → mobile dialog; content tested at 320, 375,
  768, 1024, 1440, 1920 px.
- Motion uses one easing token; all animation is wrapped in
  `@media (prefers-reduced-motion: no-preference)`.
- No `!important`; no inline styles except per-instance custom properties
  (`style="--accent: …"`).

**Core tokens (planning baseline; final values in `styles.css`):**

| Token | Value |
| --- | --- |
| `--ink` | `#07080a` |
| `--ink-2` | `#0d0f14` |
| `--bone` | `#f2efe6` |
| `--acid` | `#d7ff3e` |
| `--magenta` | `#ff3d7f` |
| `--blue` | `#5b7cff` |
| `--display` | Space Grotesk, variable 300–700 |
| `--mono` | JetBrains Mono |
| `--step-0…6` | fluid `clamp()` type scale |
| `--space-*` | 4 px-based spacing scale |
| `--ease-out` | `cubic-bezier(.16, 1, .3, 1)` |

Contrast pairs must be verified: bone on ink, ink on acid, ink on bone, magenta on ink,
and accent-on-surface combinations used for text. Decorative-only accents do not carry
meaning.

### 4.3 JavaScript (`site/assets/js/main.js`, ≤ 18 KB)

Progressive enhancement only; the page is complete without it.

1. **Boot flag** — inline script in `<head>` adds a `js` class before first paint and
   removes it if `main.js` fails to boot within 2.5 s. Prevents hidden content when the
   script fails.
2. **Mobile navigation** — `<dialog>`: `showModal()` with feature detection, close
   button, Escape, backdrop click, focus trap via native modal semantics, focus return
   to the trigger, and body scroll lock.
3. **Scroll progress** — passive scroll listener updates a progress bar (`aria-hidden`).
4. **Active nav link** — `IntersectionObserver` marks the current section with
   `aria-current="true"`; styling must not rely on colour alone.
5. **Reveal on scroll** — `IntersectionObserver` adds `is-visible`; under reduced motion
   (or without IO support) elements start visible.
6. **Copy email** — Clipboard API with `navigator.clipboard` guard, `try/catch`, visible
   success/failure message in an `aria-live="polite"` region; the `mailto:` link always
   works.
7. **Pointer spotlight** — only for `(pointer: fine)` and motion allowed; no effect on
   touch or reduced motion.
8. **Footer year** — set `textContent` to the current year.
9. **Back to top** — show after a threshold, smooth scroll only when motion is allowed.

Constraints:

- Single IIFE or one namespace; no globals, no dependencies.
- Guard every browser API; failures must degrade to the static experience.
- No layout reads/writes in scroll loops; batch with `requestAnimationFrame`.
- No content strings duplicated between HTML and JS.

### 4.4 Contact

`mailto:hassanullah510@gmail.com` link, plus a copy button. No form, no third-party
handler, no data collection.

### 4.5 Assets

| Asset | Decision |
| --- | --- |
| Fonts | Space Grotesk (variable) + JetBrains Mono, Latin subset, WOFF2, ~54 KB total, OFL licence bundled |
| Icons/favicons | Hand-authored SVG + 32/180 px PNG rendered from `tools/icon.html` |
| Social preview | 1200 × 630 PNG rendered from `tools/og-template.html` |
| Project visuals | CSS gradients, rules, and type — zero image requests |
| Decoration | CSS grain via an inline SVG data URI (no network request) |

Fonts are preloaded with `crossorigin`, declared with `font-display: swap`, and paired
with a system fallback stack to minimise CLS.

### 4.6 Metadata and SEO

- Descriptive `<title>`, meta description, author, `theme-color`, `color-scheme`.
- Canonical URL: `https://hassanullah510-source.github.io/hassan-portfolio/`.
- Open Graph + Twitter (`summary_large_image`) with the 1200 × 630 image and alt text.
- JSON-LD `Person`: name, job title, email, URL, GitHub `sameAs`, `knowsAbout`.
- `robots.txt` (points to the sitemap), `sitemap.xml` (single canonical URL),
  `site.webmanifest` (name, colours, icons).
- All absolute URLs are documented as editable in one place in the README.
- `404.html` uses absolute home/favicon URLs because it can be served from any path.

### 4.7 Deployment (GitHub Pages)

`.github/workflows/pages.yml`:

1. Trigger on push to `main` and `workflow_dispatch`.
2. Permissions: `contents: read`, `pages: write`, `id-token: write`.
3. `actions/configure-pages` → `actions/upload-pages-artifact` with `path: site` →
   `actions/deploy-pages`.
4. Concurrency group `pages` with `cancel-in-progress: true`.

Because Pages serves the site from `/<repo>/`, every internal path is relative
(`assets/css/styles.css`, `#work`) and never root-absolute (`/assets/...`).

## 5. Design direction — SIGNAL

- Editorial neo-brutalist: oversized display type, numbered sections, hard rules,
  generous negative space, monogram tile.
- Near-black ink canvas with one bone-paper section for rhythm and an acid contact block.
- Acid-lime primary, magenta and electric-blue accents; accents are decorative, never
  the sole carrier of meaning.
- Outlined display text, grain overlay, marquee ticker, hover inversion of work rows.
- Motion: staggered reveals, marquee, pointer spotlight. All decorative and disabled
  under reduced motion.
- Typography: Space Grotesk for display/body; JetBrains Mono for labels, indexes, tags,
  and metadata.

## 6. Accessibility plan (WCAG 2.2 AA)

| Area | Implementation |
| --- | --- |
| Structure | Landmarks (`header`/`nav`/`main`/`footer`), one `h1`, ordered headings, labelled sections |
| Skip link | First focusable element; visible on focus; targets `<main id="main">` |
| Keyboard | All controls are native buttons/links/`details`; logical tab order; no keyboard traps |
| Focus | Single high-contrast `:focus-visible` ring token, visible on every surface |
| Mobile menu | Native `<dialog>` modal, Escape closes, focus returns to trigger |
| Targets | Interactive controls ≥ 44 × 44 px |
| Colour | AA contrast; active nav also uses weight/underline; status has text, not just a dot |
| Motion | Everything decorative is inside `no-preference`; smooth scroll becomes instant |
| Live regions | Copy result announced politely; no unexpected focus moves |
| Zoom/reflow | Usable at 200% zoom and 320 px; no horizontal scroll |
| Images | Meaningful images have alt text; decorative graphics are `aria-hidden` |

## 7. Performance plan

| Technique | Effect |
| --- | --- |
| Self-hosted WOFF2, Latin subset, preload | Fast text render, no third-party DNS/TLS |
| `font-display: swap` + metrics-friendly fallback | Minimises invisible text and CLS |
| Deferred single JS file | No render-blocking scripts |
| CSS-only visuals (gradients, rules, type) | No image payload for project art |
| Inline SVG favicon + small PNGs | Tiny icon payload |
| Lazy-loading any below-fold raster images | Protects LCP |
| No third-party requests | Fewer connections, better privacy and Best Practices scores |

Budgets are enforced from section 8 of `spec.md` and re-measured before release with
Lighthouse mobile.

## 8. Content editing model

| Change | Where | Steps |
| --- | --- | --- |
| Project link | `site/index.html` | Replace `Link pending` with the documented anchor block; update README table and open items. |
| New project | `site/index.html` | Copy one `work-item`, update index, title id, description, tags, accent. |
| Hero/about/skills/experience/services | `site/index.html` | Edit the corresponding section; keep facts consistent with the spec's content inventory. |
| Colours/type/spacing | `site/assets/css/styles.css` | Change tokens only; do not hard-code values downstream. |
| Email / GitHub | `site/index.html`, JSON-LD | Update every occurrence; verify links. |
| Repository/domain | metadata, `robots.txt`, `sitemap.xml`, `404.html`, README | Use the README's checklist of absolute URLs. |

## 9. Validation strategy

| Check | Method | Pass condition |
| --- | --- | --- |
| HTML validity | `npx.cmd --yes vnu-jar site/index.html site/404.html` | 0 errors |
| Lighthouse | `npx.cmd --yes lighthouse http://localhost:8080 --output=html` | Per budgets (§8 of the spec) |
| Accessibility | Keyboard walkthrough + axe DevTools + contrast check | No critical/serious issues |
| Responsive | 320, 375, 768, 1024, 1440, 1920 px | No overflow; content readable |
| No-JS | Disable JavaScript | All content, nav, and links usable |
| Reduced motion | Emulate `prefers-reduced-motion: reduce` | No animation; content visible |
| Links | Manual/automated audit | No dead links; pending items labelled |
| Console | DevTools | No errors or warnings |
| Sub-path | Serve under a nested path | All assets resolve with relative URLs |

## 10. Implementation phases

1. **Scaffold** — repo layout, `site/`, `.github/workflows/pages.yml`, `.gitignore`.
2. **Content shell** — full semantic HTML for all seven sections with real copy; no JS.
3. **Design system** — tokens, reset, base typography, layout, then components.
4. **Responsive pass** — header/mobile dialog, grids, 320–1920 px.
5. **Enhancement layer** — `main.js` features from §4.3 with guards and fallbacks.
6. **Metadata and assets** — fonts, favicons, social image, SEO files, 404.
7. **Validation** — HTML, Lighthouse, keyboard, reduced motion, no-JS, links, sub-path.
8. **Docs** — README editing recipes, deployment steps, open items; update spec/plan.

Detailed ordering and acceptance per task belongs in `tasks.md`.

## 11. Risks and mitigations

| Risk | Likelihood | Mitigation |
| --- | --- | --- |
| Project URLs never supplied | High | Visible `Link pending` state; README/spec open items; no fake anchors. |
| Fonts exceed budget / cause CLS | Medium | Subset, WOFF2, preload, `swap`, fallback metrics; measure. |
| Dialog not supported / focus bugs | Low | Feature-detect `showModal()`; fall back to anchor navigation; test keyboard. |
| Clipboard API blocked | Medium | `try/catch`, visible failure message, `mailto:` remains. |
| Pages sub-path breaks assets | Medium | Relative paths only; test under a nested path before release. |
| Reduced-motion gaps | Medium | Global guard in `reset`; manual walkthrough in checklist. |
| Content drift between sections | Medium | Spec content inventory is the single source of truth. |
| Repo/domain mismatch | Medium | One documented list of absolute URLs to update. |

## 12. Complexity tracking

No constitution deviations or added complexity are planned.

Add rows here only if implementation must deviate. Each entry needs: the principle, the
reason, the alternative considered, and the owner's approval date.

| Principle | Deviation | Reason | Alternative considered | Approved |
| --- | --- | --- | --- | --- |
| — | — | — | — | — |

## 13. Open decisions

| ID | Decision | Default until answered |
| --- | --- | --- |
| D-1 | Final repository name and Pages URL | `hassan-portfolio` as assumed in the spec. |
| D-2 | Project live/repo URLs | `Link pending` labels; no anchors. |
| D-3 | LinkedIn / X profiles | Omitted from the site and JSON-LD. |
| D-4 | Photo / résumé | Out of scope for v1. |

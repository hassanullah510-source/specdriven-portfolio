# Tasks — Personal Portfolio

**Feature:** `001-portfolio`
**Spec:** [spec.md](./spec.md) · **Plan:** [plan.md](./plan.md)
**Status:** In progress — specification package complete, implementation not started
**Legend:** `[P]` can run in parallel · `[x]` complete

---

## Phase 0 — Specification package (gate)

- [x] **T001** Write `spec.md` with personas, nine user stories, acceptance criteria,
  functional and non-functional requirements, content inventory, success criteria,
  assumptions, risks, and open items.
  *Done when:* every requirement is testable and traced to an owner-confirmed fact.
- [x] **T002** Write `plan.md` with technical context, constitution check, structure,
  architecture, design tokens, JS inventory, SEO, deployment, validation, and risks.
  *Done when:* the plan satisfies all seven constitution principles with no deviations.
- [x] **T003** Add `.specify/memory/constitution.md` v1.0.0 (honest content, static
  first, accessibility, performance, design system, maintainability, privacy).
  *Done when:* the file exists at the path linked from `plan.md`.
- [ ] **T004** Write `checklist.md` covering requirements, accessibility, performance,
  responsiveness, links, and release sign-off.
  *Done when:* every functional requirement maps to at least one check.

**Gate:** do not start implementation until T003–T004 are done and the spec/plan are
reviewed by the owner.

## Phase 1 — Repository setup

- [ ] **T005** Initialise a dedicated Git repository with `main` as the default branch.
  *Done when:* `git status` runs inside this folder and the site is not tracked by the
  outer home-directory repository.
- [ ] **T006** [P] Create the folder tree: `site`, `site/assets/{css,js,img,fonts}`,
  `.github/workflows`, `tools`, `.specify/memory`.
  *Done when:* every folder in the plan's structure exists.
- [ ] **T007** [P] Add `.gitignore` for OS files, editor files, and audit output
  (`lighthouse-report.*`, `.DS_Store`, `Thumbs.db`, `.vscode/`).
- [ ] **T008** [P] Obtain the Latin-subset WOFF2 files for Space Grotesk and JetBrains
  Mono, plus `OFL.txt`, into `site/assets/fonts/`.
  *Done when:* both font files exist and total ≤ 100 KB.
- [ ] **T009** [P] Record the final repository name and Pages URL decision in
  `spec.md` §10 and `plan.md` §13 (default: `hassan-portfolio`).

## Phase 2 — Foundation (HTML + design system)

- [ ] **T010** Create `site/index.html` skeleton: doctype, `lang`, metadata placeholders,
  skip link, header, mobile dialog, main, footer, and the seven section landmarks.
  *Done when:* the document validates and the section order matches the plan.
- [ ] **T011** Write `styles.css` layers `reset, tokens, base` with colour, type scale,
  spacing, radius, shadow, easing, and container tokens.
  *Done when:* no component rule hard-codes a colour or spacing value.
- [ ] **T012** Add self-hosted `@font-face` declarations with `font-display: swap`,
  preload links, and a system fallback stack.
- [ ] **T013** Implement the fixed header: wordmark/monogram, desktop nav, availability
  status, primary CTA, and scroll progress bar.
- [ ] **T014** Implement the mobile navigation `<dialog>` markup, styles, and initial
  closed state (no behaviour yet).
- [ ] **T015** Implement shared components: buttons (acid/ghost), eyebrow labels,
  section headings, tags/chips, pending labels, and the focus-visible ring.
- [ ] **T016** Verify the page is complete and readable with CSS only; no JS-dependent
  content or hidden sections.
  *Done when:* all seven sections and all links render without JavaScript.

## Phase 3 — Content sections

- [ ] **T017** Build the hero: name, role, value statement, availability, two CTAs,
  "at a glance" panel, and decorative glow.
  *Done when:* name, role, and primary CTA are visible above the fold at 360 × 640.
- [ ] **T018** Build the marquee/ticker strip with duplicated `aria-hidden` content and
  a reduced-motion pause.
- [ ] **T019** Build the six project rows: index, title, one-sentence description,
  stack tags, accent variable, and `Link pending` state.
  *Done when:* no project has an `href="#"` or placeholder URL.
- [ ] **T020** Build the about section: statement, facts grid, and generated monogram
  (no photo).
  *Done when:* every fact traces to `spec.md` §9.
- [ ] **T021** Build the skills section: three semantic `<ul>` groups (Frontend,
  Backend, Tooling), each with a visible heading.
  *Done when:* no percentage bars or proficiency scores exist.
- [ ] **T022** Build the experience section: the "6+ years" statement and four
  capability phases with no employers or dates.
- [ ] **T023** Build the services accordion: seven native `<details>` entries, first
  open by default, each with an `h3`-equivalent summary.
- [ ] **T024** Build the contact section: large `mailto:` link, copy button, status live
  region, GitHub link, and availability note.
- [ ] **T025** Build the footer: quick links, copyright/year placeholder, back-to-top
  anchor/button, and credits line.
- [ ] **T026** Audit copy against `spec.md` §9: every claim is real, consistent, and
  owns its open item where missing.

## Phase 4 — Design and responsive pass

- [ ] **T027** Implement the SIGNAL visual language: oversized display type, outlined
  text, numbered sections, hard rules, grain overlay, and accent blocks.
- [ ] **T028** Implement the one bone-paper section and acid contact block for rhythm.
- [ ] **T029** Implement responsive grids and fluid type; no horizontal scrolling at
  320 px.
- [ ] **T030** Implement the breakpoint at ~760 px: desktop nav swaps to the mobile
  menu trigger.
- [ ] **T031** Verify contrast for every text/accent pairing (4.5:1 body, 3:1 large/UI)
  and fix failures in tokens, not components.
- [ ] **T032** Verify tap targets are ≥ 44 × 44 px on mobile.

## Phase 5 — Enhancement layer (`main.js`)

- [ ] **T033** Implement the bootstrap: early `js` class (inline in `<head>`), boot-flag
  timeout, reduced-motion query, and shared helpers.
- [ ] **T034** Implement the mobile dialog: `showModal()` feature detect, open/close,
  Escape, backdrop click, scroll lock, focus trap, and focus return.
- [ ] **T035** Implement scroll progress and active navigation with
  `IntersectionObserver`; active state must not rely on colour alone.
- [ ] **T036** Implement reveal-on-scroll with stagger; elements are immediately visible
  under reduced motion or without `IntersectionObserver`.
- [ ] **T037** Implement copy-email with Clipboard API guards, success/failure message
  in an `aria-live="polite"` region, and a working `mailto:` fallback.
- [ ] **T038** Implement pointer spotlight (fine pointers + motion allowed only), footer
  year, and back-to-top visibility.
- [ ] **T039** Audit all handlers for guards and failures; confirm no console errors and
  every enhancement degrades cleanly.
  *Done when:* disabling JavaScript leaves all content and links usable.

## Phase 6 — Assets and metadata

- [ ] **T040** Create the SVG favicon, 32 px and 180 px PNGs, and `site.webmanifest`.
- [ ] **T041** Create the 1200 × 630 social preview from `tools/og-template.html`
  (headless Chrome) and keep the source template in `tools/`.
- [ ] **T042** Add `<title>`, meta description, author, `theme-color`,
  `color-scheme`, and canonical URL.
- [ ] **T043** Add Open Graph and Twitter (`summary_large_image`) tags with image
  dimensions and alt text.
- [ ] **T044** Add JSON-LD `Person` (name, job title, email, URL, GitHub `sameAs`,
  `knowsAbout`) matching visible content.
- [ ] **T045** Write `robots.txt` and `sitemap.xml` with the documented canonical URL.
- [ ] **T046** Build `404.html` matching the design with absolute home/favicon URLs.
- [ ] **T047** Verify no third-party requests: fonts, icons, images, and scripts are all
  local.

## Phase 7 — Documentation and deployment

- [ ] **T048** Write `README.md`: overview, structure, local preview, content editing
  recipes, project-link workflow, open items, and credits.
- [ ] **T049** Add `.github/workflows/pages.yml`: configure Pages, upload `site/`,
  deploy, with `pages: write` and `id-token: write`.
- [ ] **T050** Document the exact GitHub steps: create repo, push, set Pages source to
  **GitHub Actions**, watch the Actions tab.
- [ ] **T051** Add the six-project link table (project, live URL, repository) to the
  README with every URL marked pending or real.
- [ ] **T052** Document every place to update if the repository name or domain changes.

## Phase 8 — Validation

- [ ] **T053** Validate HTML with `npx.cmd --yes vnu-jar site/index.html site/404.html`;
  fix all errors.
- [ ] **T054** Run Lighthouse mobile against a local server; record scores and confirm
  Performance ≥ 95, Accessibility 100, Best Practices ≥ 95, SEO 100.
- [ ] **T055** Keyboard walkthrough: skip link, primary nav, mobile dialog, accordion,
  copy button, back to top.
- [ ] **T056** Reduced-motion walkthrough: no marquee, reveals, smooth scroll, or pointer
  effects; content visible.
- [ ] **T057** No-JS walkthrough: all content, navigation, and links usable.
- [ ] **T058** Responsive check at 320, 375, 768, 1024, 1440, and 1920 px; no overflow.
- [ ] **T059** 200% zoom check; all content and actions remain available.
- [ ] **T060** Automated accessibility check (axe DevTools) and contrast audit; no
  critical/serious issues.
- [ ] **T061** Link audit: every link resolves or shows `Link pending`; no dead anchors.
- [ ] **T062** Confirm zero console errors or warnings in production mode.
- [ ] **T063** Serve under a nested path (simulating `/<repo>/`) and confirm all assets
  resolve.
- [ ] **T064** Verify page weight (HTML + CSS + JS + fonts) ≤ 220 KB excluding the social
  image.

## Phase 9 — Release

- [ ] **T065** Review and tick every applicable item in `checklist.md`.
- [ ] **T066** Update `spec.md` and `plan.md` status to Implemented; record any
  deviations in `plan.md` §12.
- [ ] **T067** Commit the completed site with a descriptive message and push `main`.
- [ ] **T068** Confirm the Pages deployment succeeds and the live URL loads correctly.
- [ ] **T069** Re-check the live site for broken relative assets, mixed content, and
  correct canonical/OG URLs.

---

## Requirement traceability

| Requirement | Covered by |
| --- | --- |
| US1 / FR-001, FR-002, FR-014 | T010, T013, T017, T025 |
| US2 / FR-004, FR-005 | T019, T026, T061 |
| US3 / FR-006, FR-007 | T021, T022, T026 |
| US4 / FR-008 | T023 |
| US5 / FR-009, FR-010, FR-018 | T024, T037 |
| US6 / FR-002, FR-003, FR-017 | T013, T014, T029, T030, T034, T055 |
| US7 / FR-015 | T018, T036, T038, T056 |
| US8 / FR-004 | T019, T048, T050, T051, T052 |
| US9 / FR-011, FR-012 | T040–T046 |
| FR-013 relative URLs | T009, T063 |
| FR-016 self-hosted fonts | T008, T012, T047 |
| NFR-001, NFR-011 (accessibility, contrast) | T031, T032, T055, T056, T060 |
| NFR-002, NFR-008 (performance, weight) | T054, T064 |
| NFR-003, NFR-009 (responsive, zoom) | T029, T058, T059 |
| NFR-004, NFR-005 (browsers, HTML) | T053, T058 |
| NFR-006, NFR-007 (no-JS, no third parties) | T016, T039, T047, T057 |
| NFR-010 (console clean) | T039, T062 |
| NFR-012 (reduced motion) | T018, T036, T038, T056 |

## Dependencies

- T003–T004 precede all implementation (spec-driven gate).
- T005–T009 precede file creation.
- T010–T016 precede T017–T026.
- T017–T026 precede T027–T032 (design pass) and T033–T039 (enhancements).
- T040–T047 follow the content sections so metadata matches final copy.
- T048–T052 can run in parallel with T040–T047.
- T053–T064 run after all content, design, and enhancements.
- T065–T069 are last.

## Parallel opportunities

- T006, T007, T008, T009 (setup).
- T040, T041, T045, T046 (static assets/metadata) once copy is final.
- T048, T049, T050, T052 (docs/deploy) alongside Phase 6.
- T055, T056, T057, T058, T060 (manual checks) by different reviewers if available.

# Implementation Plan: Portfolio Website

## Overview

Build a single-page portfolio website using HTML5, CSS3, and vanilla JavaScript (ES2020+). The implementation proceeds module by module — design tokens and base styles first, then each section in order, followed by interactive JS modules and tests. All modules are wired together in `main.js` at the end.

## Tasks

- [-] 1. Project scaffold and design tokens
  - Create the full directory structure: `assets/fonts`, `assets/images`, `assets/icons`, `css/`, `js/`, `tests/`
  - Create `index.html` with semantic HTML5 shell (`<header>`, `<main>`, `<footer>`, `<nav>`) and all six section skeletons (`#home`, `#about`, `#services`, `#portfolio`, `#testimonials`, `#contact`)
  - Create `css/tokens.css` with CSS custom properties for the black-and-white palette, spacing scale, border-radius (≥ 8px), and transition durations (150–300 ms)
  - Create `css/base.css` with CSS reset and typography baseline (≥ 16px body, max two typefaces)
  - Create `css/layout.css` with CSS Grid/flex utilities and the three responsive breakpoints (< 768 px, 768–1023 px, ≥ 1024 px)
  - Create `css/components.css` for card, button, and form component styles (border-radius ≥ 8px, CSS transitions on interactive elements)
  - Create `css/animations.css` with `@keyframes` definitions and `.is-visible` utility class; include `@media (prefers-reduced-motion: reduce)` block that disables all entrance transitions
  - Link all CSS files in `index.html` in correct cascade order
  - _Requirements: 8.1, 8.2, 8.3, 8.4, 8.5, 10.3_

- [ ] 2. Navigation bar and smooth scrolling
  - [~] 2.1 Implement `js/navigation.js` — `initNavigation`, `scrollToSection`, and `toggleMobileMenu` functions
    - Sticky nav via `position: sticky; top: 0; z-index: 100`
    - Add all six section links to the `<nav>` in `index.html`
    - Hamburger toggle visible only when viewport width < 768 px
    - On nav link click: call `scrollToSection(id)` and close mobile menu if open
    - Hover effect via CSS transition on link `color` (200 ms, defined in `components.css`)
    - _Requirements: 1.1, 1.2, 1.3, 1.4, 1.5, 1.6_

  - [ ]* 2.2 Write unit tests for navigation module (`tests/navigation.test.js`)
    - Test hamburger toggle state machine (open/close/toggle)
    - Test `scrollToSection` resolves correct element id
    - Test nav link click closes mobile menu
    - _Requirements: 1.3, 1.4, 1.5_

- [~] 3. Hero section
  - Add Hero HTML content to `#home` in `index.html`: Owner name, professional title, tagline, and at least one CTA button pointing to `#portfolio` or `#contact`
  - Add `heroEntrance` keyframe animation in `css/animations.css` (fade-in + slide-up, ≤ 800 ms); trigger via `js/main.js` on DOMContentLoaded
  - Apply hover effect to CTA button via CSS transition in `components.css` (200 ms)
  - Ensure Hero content fits within initial viewport without scrolling (100vh section)
  - _Requirements: 2.1, 2.2, 2.3, 2.4, 2.5_

- [~] 4. About Me section
  - Add About Me HTML to `#about` in `index.html`: biography paragraph, skills/expertise summary, and Owner photo `<img>` with descriptive `alt` attribute
  - Add `[data-animate]` attribute to About section elements for IntersectionObserver targeting
  - Implement two-column (photo | text) to single-column responsive layout in `css/layout.css` using CSS Grid `auto-fit` (breakpoint < 768 px)
  - Entrance animation: fade-in/slide (≤ 600 ms) via `.is-visible` class in `css/animations.css`
  - _Requirements: 3.1, 3.2, 3.3, 3.4, 9.3_

- [ ] 5. Services section
  - [~] 5.1 Build Services section HTML and CSS
    - Add Services HTML to `#services` in `index.html` with a minimum of three service cards, each containing `title`, inline SVG icon, and `description`
    - Mark each service card with `[data-animate]` and `[data-stagger-index]` attributes
    - Style service cards in `css/components.css`: rounded corners (≥ 8px), hover `translateY(-4px)` + `box-shadow` elevation (200 ms transition)
    - Implement single-column layout for viewport < 768 px in `css/layout.css`
    - _Requirements: 4.1, 4.2, 4.3, 4.5_

  - [~] 5.2 Implement staggered entrance animation for service cards in `css/animations.css`
    - Use `nth-child` selectors to apply incrementally increasing `animation-delay` values: `n × BASE_STAGGER_MS`
    - The stagger is CSS-only; `js/animations.js` triggers the class addition via IntersectionObserver
    - _Requirements: 4.4_

  - [ ]* 5.3 Write property test for service item rendering (`tests/services.test.js`)
    - **Property 5: Service items render required fields**
    - **Validates: Requirements 4.1**
    - Use `fc.record({ id, title, iconSvg, description })` arbitrary; assert title, icon reference, and description appear in rendered HTML

- [ ] 6. Portfolio section
  - [~] 6.1 Build Portfolio section HTML, CSS, and data
    - Add Portfolio HTML to `#portfolio` in `index.html` with a minimum of three Portfolio_Items; each item has a project `<img>` (with `imageAlt`), `title`, and `category` tag
    - Define `PortfolioItem` data objects in `js/portfolio.js`
    - Implement responsive CSS Grid in `css/layout.css`: 1 col < 768 px, 2 col 768–1023 px, 3 col ≥ 1024 px
    - Implement hover overlay (CSS `::after` pseudo-element, `opacity: 0 → 1`, ≤ 300 ms) showing title and "View" label
    - Mark items with `[data-animate]` for entrance animation
    - _Requirements: 5.1, 5.2, 5.5, 5.6, 5.7, 9.3_

  - [~] 6.2 Implement Portfolio modal (`js/portfolio.js`) — `initPortfolio`, `openModal`, `closeModal`
    - `openModal(itemId)`: renders modal overlay with full project description; traps keyboard focus (Tab / Shift+Tab cycling)
    - ESC key closes modal; `closeModal()` restores focus to triggering Portfolio_Item
    - _Requirements: 5.3, 9.4_

  - [ ]* 6.3 Write property test for portfolio item rendering (`tests/portfolio.test.js`)
    - **Property 6: Portfolio items render required fields**
    - **Validates: Requirements 5.1, 9.3**
    - Use `fc.record({ id, title, category, imageSrc, imageAlt, description, tags })` arbitrary; assert title, imageAlt, and category appear in rendered HTML

  - [ ]* 6.4 Write unit tests for portfolio modal (`tests/portfolio.test.js`)
    - Test modal opens with correct item data
    - Test ESC key closes modal
    - Test focus is trapped within open modal (Tab cycling)
    - Test focus returns to triggering item on close
    - _Requirements: 5.3, 9.4_

- [ ] 7. Testimonials section
  - Add Testimonials HTML to `#testimonials` in `index.html` with a minimum of three Testimonial_Cards; each card contains `quote`, `clientName`, and `clientRole`
  - Style cards in `css/components.css`: rounded corners (≥ 8px)
  - Implement single-column or CSS Scroll Snap horizontal carousel layout for viewport < 768 px
  - Mark cards with `[data-animate]` for entrance fade-in transition
  - _Requirements: 6.1, 6.2, 6.3, 6.4_

  - [ ]* 7.1 Write property test for testimonial card rendering (`tests/portfolio.test.js`)
    - **Property 7: Testimonial cards render required content**
    - **Validates: Requirements 6.1**
    - Use `fc.record({ quote, clientName, clientRole })` arbitrary; assert all three strings appear in rendered HTML

- [~] 8. Checkpoint — Core sections complete
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 9. Contact section
  - [~] 9.1 Build Contact section HTML and Owner info
    - Add Contact HTML to `#contact` in `index.html`: Contact_Form with `name`, `email`, `subject`, and `message` fields; Owner email address and social/professional profile links alongside the form
    - Wire `aria-describedby` on each input pointing to its inline error `<span>`
    - Apply visible focus indicator styles in `css/components.css`
    - _Requirements: 7.1, 7.5, 7.6_

  - [~] 9.2 Implement `js/contact.js` — `initContactForm`, `validateField`, `validateForm`, `submitForm`
    - `validateField(fieldName, value)`: pure function, returns `ValidationResult`; enforces rules from design (name 1–100 chars, email RFC 5322 simplified, subject 1–200 chars, message 1–2000 chars); whitespace-only strings fail
    - `validateForm(formData)`: calls `validateField` for each field; returns array of `ValidationResult`
    - `initContactForm()`: attaches submit listener; runs `validateForm` on submit; shows inline errors without page reload; on success shows confirmation message; on network error shows retry banner and re-enables submit button
    - `submitForm(formData)`: POSTs to Formspree/EmailJS endpoint; resolves `SubmitResult`
    - _Requirements: 7.2, 7.3, 7.4_

  - [ ]* 9.3 Write property test: whitespace-only inputs fail validation (`tests/contact.test.js`)
    - **Property 1: Non-whitespace field validation rejects blank-equivalent inputs**
    - **Validates: Requirements 7.3, 7.4**
    - Use `fc.stringMatching(/^\s+$/)` arbitrary for each required field; assert `validateField(f, v).valid === false`

  - [ ]* 9.4 Write property test: valid form data passes all validations (`tests/contact.test.js`)
    - **Property 2: Valid contact form data passes all field validations**
    - **Validates: Requirements 7.2**
    - Use composed valid-field arbitraries; assert every `ValidationResult.valid === true`

  - [ ]* 9.5 Write property test: invalid email always fails (`tests/contact.test.js`)
    - **Property 3: Invalid email format always fails email validation**
    - **Validates: Requirements 7.4**
    - Use `fc.string()` filtered to strings without `@`; assert `validateField('email', v).valid === false`

  - [ ]* 9.6 Write property test: partial form always fails (`tests/contact.test.js`)
    - **Property 4: Partial form submission always fails**
    - **Validates: Requirements 7.3**
    - Use `ContactFormData` arbitrary with at least one blank field; assert at least one `ValidationResult.valid === false` with non-null `error`

  - [ ]* 9.7 Write unit tests for contact form (`tests/contact.test.js`)
    - Test success path: valid submission hides form and shows confirmation message
    - Test error path: non-2xx response shows retry banner and re-enables submit button
    - Test inline validation errors appear on submit with missing fields
    - _Requirements: 7.2, 7.3, 7.4_

- [ ] 10. Animations module
  - [~] 10.1 Implement `js/animations.js` — `initAnimations` and `prefersReducedMotion`
    - `prefersReducedMotion()`: returns `window.matchMedia('(prefers-reduced-motion: reduce)').matches`
    - `initAnimations()`: if `prefersReducedMotion()` is true, immediately add `.is-visible` to all `[data-animate]` elements and skip IntersectionObserver; otherwise observe each element, add `.is-visible` on first intersection, then unobserve
    - _Requirements: 10.1, 10.2, 10.3, 10.4_

  - [ ]* 10.2 Write property test: stagger delay proportional to index (`tests/animations.test.js`)
    - **Property 8: Staggered animation delay is proportional to item index**
    - **Validates: Requirements 4.4, 5.4**
    - Use `fc.integer({ min: 0, max: 20 })` as index; assert computed delay equals `index × BASE_STAGGER_MS`

  - [ ]* 10.3 Write property test: animation fires exactly once per element (`tests/animations.test.js`)
    - **Property 9: Animation entrance fires exactly once per element**
    - **Validates: Requirements 10.1, 10.4**
    - Simulate multiple IntersectionObserver callbacks for the same element; assert `.is-visible` is added exactly once and element is unobserved after first intersection

  - [ ]* 10.4 Write property test: reduced-motion disables all entrance animations (`tests/animations.test.js`)
    - **Property 10: Reduced-motion disables all entrance animations**
    - **Validates: Requirements 10.3**
    - Set `prefersReducedMotion = true`; call `initAnimations()`; assert all `[data-animate]` elements immediately have `.is-visible` without IntersectionObserver being used

- [ ] 11. Wire everything together in `main.js`
  - [~] 11.1 Create `js/main.js` orchestrator
    - Import and call `initNavigation()`, `initAnimations()`, `initPortfolio()`, `initContactForm()` on `DOMContentLoaded`
    - Trigger Hero entrance animation sequence
    - Add top-level `window.addEventListener('error', ...)` handler to prevent non-critical module failures from breaking the page
    - _Requirements: 1.3, 2.3, 9.4_

  - [~] 11.2 Add image error handling and accessibility attributes
    - Add `onerror` fallback or CSS background placeholder on all `<img>` elements in `index.html`
    - Verify all images have descriptive `alt` text (required by `PortfolioItem.imageAlt`)
    - Verify all interactive elements are reachable and activatable via Tab and Enter
    - _Requirements: 9.2, 9.3, 9.4_

  - [ ]* 11.3 Write accessibility unit tests
    - Test all images have non-empty `alt` attributes
    - Test all form inputs have associated `aria-describedby` error spans
    - Test interactive elements (nav links, buttons, modal triggers) are in keyboard focus order
    - _Requirements: 9.2, 9.3, 9.4_

- [~] 12. Final checkpoint — Ensure all tests pass
  - Ensure all tests pass, ask the user if questions arise.

## Notes

- Tasks marked with `*` are optional and can be skipped for faster MVP
- Each task references specific requirements for traceability
- Checkpoints (tasks 8 and 12) ensure incremental validation
- Property tests (Properties 1–10) validate universal correctness properties using fast-check with ≥ 100 iterations per property
- Unit tests validate specific examples, edge cases, and DOM interactions
- All CSS animations are independently suppressed by `@media (prefers-reduced-motion: reduce)` — this works even without JavaScript

## Task Dependency Graph

```json
{
  "waves": [
    { "id": 0, "tasks": ["1"] },
    { "id": 1, "tasks": ["2.1", "3", "4", "5.1"] },
    { "id": 2, "tasks": ["2.2", "5.2", "5.3", "6.1", "7"] },
    { "id": 3, "tasks": ["6.2", "6.3", "7.1", "9.1"] },
    { "id": 4, "tasks": ["6.4", "9.2", "10.1"] },
    { "id": 5, "tasks": ["9.3", "9.4", "9.5", "9.6", "9.7", "10.2", "10.3", "10.4"] },
    { "id": 6, "tasks": ["11.1"] },
    { "id": 7, "tasks": ["11.2"] },
    { "id": 8, "tasks": ["11.3"] }
  ]
}
```

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Single-page bridal photography marketing + booking site (Elysian Bridal Photography). The entire site is **one self-contained file**: [index.html](index.html). No build system, no dependencies, no tests.

## Running locally

There is no install step. Serve the directory over HTTP:

```bash
python -m http.server 8000
# then open http://localhost:8000/index.html
```

Use `localhost` rather than `file://` so the booking form's `fetch` call to FormSubmit works without CORS quirks and Google Fonts load reliably.

## Architecture

Everything lives in `index.html` in this order — keep this layering when editing:

1. `<head>` — meta + Google Fonts (`Playfair Display`, `Cormorant Garamond`, `Great Vibes`).
2. Inline `<style>` (lines ~15–625) — palette variables, components, mobile-first layout, one `@media (min-width: 768px)` breakpoint.
3. `<header class="nav">` (~line 629) — fixed nav, toggles `.scrolled` class past 40px scroll, separate `.mobile-menu` panel.
4. `<main>` (~line 656) — sections in order: Hero → About → Packages → Gallery → Booking → Contact.
5. `<footer>` (~line 906).
6. Inline `<script>` (~line 912) — all behavior.

### Styling conventions

- All colors come from CSS custom properties on `:root` (`--blush`, `--rose`, `--gold`, `--ivory`, `--ink`, etc.). **Edit the palette there**, not at individual call sites.
- Fluid typography uses `clamp()`. Avoid fixed `px` font sizes.
- Mobile-first; desktop layout shifts (multi-column grids, visible nav links, hidden hamburger) live inside the single 768px media query.
- Scroll-fade animations are opt-in via the `.reveal` class — see "JS behaviors" below. They respect `prefers-reduced-motion`.

### JS behaviors (all in the inline `<script>`)

- **Nav scroll state** — adds `.scrolled` to `#nav` past 40px.
- **Mobile menu** — `#menuBtn` toggles `.open` on `#mobileMenu` and updates `aria-expanded`.
- **Scroll reveal** — `IntersectionObserver` adds `.visible` to elements with class `reveal`. To make new elements fade in, give them `class="reveal"`.
- **Package preselect** — buttons in the Packages section carry `data-pkg="Essential|Signature|Luxury"`. Clicking sets `#package` `<select>`, smooth-scrolls to `#booking`, focuses `#name`. New package cards must use the same `data-pkg` mechanism and the values must match the `<option value="...">` strings.
- **Date min** — `#date` input has `min` set to today's local date on load.
- **Form submission** — see below.

### Booking form → FormSubmit

The form posts JSON to `https://formsubmit.co/ajax/kelvinissey@gmail.com`. Important details:

- Submission is treated successful only when `res.ok && result.success === 'true'` (FormSubmit returns the success flag as a **string**).
- The payload includes `_subject`, `_template: 'table'`, `_captcha: 'false'`, and a honeypot field named `_honey` (the visible input is `name="_honey"` inside the `.honeypot` container).
- **First-submission gotcha**: the very first POST to a new recipient address returns a confirmation prompt instead of `success: "true"` until the owner of `kelvinissey@gmail.com` clicks the FormSubmit activation email. Until then, manual testing will hit the error path — that's expected, not a bug in the form code.
- On success the `<form>` is hidden and `#successCard` is shown; on failure an inline `#formError` banner appears and the submit button re-enables.

### External resources

The page relies on the network for Google Fonts and Unsplash images (`images.unsplash.com/photo-...`). Offline previews will look unstyled and image-less; this is by design — there are no local assets.

## Adding a new section

1. Add `<section id="..." aria-labelledby="...-h">` in the right place inside `<main>`, with a `.section-head` containing an `.eyebrow`, `<h2>`, and `.divider` to match existing visual rhythm.
2. Add the section's nav link to **both** `.nav-links` (desktop) and `.mobile-menu ul` (mobile) — they are separate lists.
3. Apply `class="reveal"` to the elements that should fade in on scroll.

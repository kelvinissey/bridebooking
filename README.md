# Elysian Bridal Photography

A single-page bridal photography marketing and booking website. The entire site is one self-contained `index.html` — no build step, no dependencies, no framework.

## Features

- **Hero** with full-viewport bridal imagery and dual CTAs
- **About** section with portrait and signature
- **Packages** — three pricing tiers (Essential / Signature / Luxury) with click-to-prefill booking
- **Gallery** — responsive CSS grid of bridal photography
- **Booking form** — full client-side validation, posts JSON to FormSubmit, inline success/error states
- **Contact** with email, phone, studio address, and social links
- Mobile-first responsive design with a single 768px breakpoint
- Scroll-reveal animations that respect `prefers-reduced-motion`
- Accessible: semantic landmarks, ARIA labels, WCAG-AA contrast

## Stack

- Plain HTML, CSS, JavaScript — all inline in [index.html](index.html)
- Google Fonts: Playfair Display, Cormorant Garamond, Great Vibes
- Unsplash for placeholder photography
- [FormSubmit](https://formsubmit.co) for form handling (no backend required)

## Running locally

```bash
python -m http.server 8000
```

Then open http://localhost:8000/index.html.

Use `localhost` rather than opening the file directly (`file://`) so the booking form's `fetch` works without CORS quirks and Google Fonts load reliably.

## Booking form

The form posts JSON to `https://formsubmit.co/ajax/<your-email>`. To point it at a different inbox, edit the `fetch` URL in the inline `<script>` near the bottom of `index.html`.

> **First-submission gotcha:** the very first POST to a new recipient address returns a confirmation prompt instead of `success: "true"` until the inbox owner clicks FormSubmit's activation email. Until then, manual testing will hit the error path — that's expected, not a bug.

## Customizing

- **Palette** — edit the CSS custom properties on `:root` in the inline `<style>` (blush, rose, gold, ivory, ink).
- **Packages** — update the three `.package-card` blocks; the `data-pkg` attribute on each "Book This Package" button must match an `<option value="...">` in the booking form's `<select id="package">`.
- **Images** — swap the Unsplash URLs for hero, about, and gallery sections.
- **Adding a section** — add the nav link to **both** `.nav-links` (desktop) and `.mobile-menu ul` (mobile); they are separate lists.

See [CLAUDE.md](CLAUDE.md) for deeper architectural notes.

## License

MIT

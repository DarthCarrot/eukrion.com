# Eukrion — website

Single-file static site for **Eukrion Oy (in formation)**. No build step, no framework, no dependencies. Everything lives in `index.html`; the only external requests are the two Google Fonts families (Inter and Jost).

## Files

| File | Purpose |
|---|---|
| `index.html` | The entire site — markup, styles and behaviour |
| `favicon.svg` | Browser icon |
| `og-image.jpg` | 1200×630 social preview (LinkedIn, X, WhatsApp, Slack) |
| `hero-1920.webp` / `hero-1440.webp` / `hero-1000.webp` | Hero photograph, responsive widths (WebP) |
| `hero-1920.jpg` | Hero photograph, fallback for browsers without WebP |
| `hero-portrait.webp` / `hero-portrait.jpg` | Portrait crop served to phones (≤640px) |
| `cells-band-*.webp` / `.jpg` | Cell imagery for the full-bleed band between the gap and the platform |
| `lake-dusk-*.webp` / `.jpg` | Dusk landscape behind the dark technology section |
| `cells-field-*.webp` / `.jpg` | Cell field behind the footer |
| `CNAME` | Custom domain for GitHub Pages — currently `eukrion.com` |
| `.nojekyll` | Stops GitHub trying to run Jekyll over the repo |
| `robots.txt`, `sitemap.xml` | Basic search-engine hygiene |

## Deploy to GitHub Pages

1. Create a public repository, e.g. `darthcarrot/eukrion`.
2. Push these files to the root of the `main` branch.
   ```bash
   git init
   git add .
   git commit -m "Eukrion site"
   git branch -M main
   git remote add origin https://github.com/darthcarrot/eukrion.git
   git push -u origin main
   ```
3. In the repo: **Settings → Pages → Source = Deploy from a branch**, branch `main`, folder `/ (root)`.
4. Under **Custom domain**, enter `eukrion.com` and tick **Enforce HTTPS** once the certificate is issued.

## DNS (GoDaddy)

At the registrar for `eukrion.com`:

- Four `A` records for the apex `@`, pointing to:
  `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
- One `CNAME` for `www` → `darthcarrot.github.io`

Propagation is usually minutes, occasionally a few hours. HTTPS becomes available after GitHub provisions the certificate.

## Before going live — three things to set

1. **Contact form endpoint.** `index.html` contains `https://formspree.io/f/YOUR_FORM_ID`. Create a Formspree form for Eukrion (do not reuse the Nordic Innovation Group endpoint) and replace that string. Until it is replaced the form falls back to a plain POST and will not work — so set it, or delete the form and leave the email link.
2. **Email address.** `hello@eukrion.com` appears in the footer and in the form error message. Replace with the address that actually receives mail.
3. **Domain.** If the site will live somewhere other than `eukrion.com`, update `CNAME`, `sitemap.xml`, `robots.txt` and the two `og:` meta tags.

## Replacing the hero photograph

Regenerate all six files from one high-resolution original, keeping the same names, and nothing else needs to change. Widths: 1920, 1440 and 1000 as WebP at quality 80; 1920 as progressive JPEG at quality 82; and a ~3:4 portrait crop at 900px wide in both formats. The blurred placeholder that shows while the photo loads is a base64 JPEG inside the `.hero-art` CSS rule — regenerate it from a 28×12 downscale of the new image.

Three more photographs sit lower on the page: `.band-img` (the full-bleed statement band, with the same `.sc` scrim pattern as the hero), `.bgphoto` (the landscape behind the technology section — `img` opacity and the `::after` gradient control how far forward it comes) and `.foot-bg` (the footer). All three are lazy-loaded and decorative, so they carry empty `alt` attributes.

Two CSS controls govern how the hero photo sits behind the text: `.hero-scrim` is the pale wash over the left column (raise its opacity stops if a busier image makes the headline hard to read) and `.hero-fade` blends the bottom of the photo into the page background.

## Editing content

All the content that changes lives in two places:

- **Prose** — directly in the HTML body, section by section, in reading order.
- **Interactive content** — two arrays in the `<script>` block:
  - `BRIEFS` — the rotating 07:30 briefing scenarios in the hero console
  - `LAYERS` — the seven architecture layers

Adding a layer means adding one row to `LAYERS`; the list and detail panel rebuild themselves.

## Scope

Five sections: the gap, the platform, technology, regulatory, contact. Around 730 words. Market sizing, financial projections, the phased roadmap, the module-by-module workflow detail and the round mechanics deliberately live in the business plan sent on request, not on the public page. Resist adding them back — the site's job is to make the right people ask for the document.

## Design system

Taken from the Eukrion mood board.

| Token | Hex | Use |
|---|---|---|
| Deep navy | `#16232F` / `#0E1821` | Dark bands, hero, footer, wordmark |
| Arctic blue | `#7E9CB4` | Low flags, aurora, focus states |
| Mist | `#D5DFE7` / `#E9EFF3` | Text on navy, pale band |
| Sage | `#8FAE9B` | The single accent — selection, escalation, focus ring |
| Warm gray | `#C7C4BD` | Reserved for future print/collateral |
| Off white | `#F4F5F3` | Page background |

Type: **Jost** (light, widely tracked) for the wordmark and numerals; **Inter** for everything else.

## Notes

- The hero console is an *illustrative* animation, labelled as such on the page. The briefing scenarios are fictional. Keep that label if the animation stays.
- All artwork is generated SVG in `index.html` — no image files, no stock photography, nothing to license. The cell-cluster and frosted-ring motifs are defined once as `<symbol id="cells">` and `<symbol id="torus">` near the top of the body and reused everywhere via `<use>`. To restyle them globally, edit the `gCell` / `gCellB` gradients.
- The hero landscape is one SVG. Its intensity is controlled by the `veil` gradient — lower the stop opacities to make the scene more prominent, raise them to push it back.
- The site deliberately carries no confidential figures — no round size, no projections, no market sizing, no cap table.
- No individuals are named while the company is in formation. When Eukrion Oy is incorporated, a short founders block belongs in the contact section.
- Accessibility: keyboard focus is visible, tabs and the drawer expose ARIA state, and `prefers-reduced-motion` disables the animation and reveals.

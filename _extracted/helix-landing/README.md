# Helix — AI Data Automation Platform (Landing Page)

Premium, responsive SaaS landing page built for the Phase 1 speed run.
**Zero runtime dependencies** — no UI kit, no animation engine. Plain semantic
HTML, custom CSS, and vanilla JS. Everything (bento, accordion, pricing engine,
dropdown, loader) is written from scratch.

## Run locally
No build step. Serve the folder over HTTP (modules/relative paths need a server):

```bash
npx serve .
# or:  python3 -m http.server 5173
```

Then open the printed URL.

## Stack
- Semantic HTML5 (`<header> <main> <section> <footer>`, landmarks, headings)
- Custom CSS with a single design-token block (`:root` in `styles.css`)
- Vanilla JS (`main.js`)

## Feature 1 — Matrix-driven pricing & isolated currency switcher
- Prices are **computed**, never hardcoded. Source of truth is the `MATRIX`
  object in `main.js`:
  `monthly = baseUSD × currency.rate × currency.tariff`, then a flat **20%**
  annual-discount multiplier. Supports **INR ₹ / USD $ / EUR €**.
- **State isolation:** the amount `<span>`s are cached once on load. Changing the
  billing toggle or currency only runs `el.textContent = …` on those spans —
  the parent cards, sections, and layout are never re-rendered.

### How to verify the isolation (for evaluation)
1. Chrome DevTools → **Rendering** → enable **Paint flashing**.
2. Toggle billing / switch currency.
3. Only the price numbers repaint. No surrounding block flashes → no global reflow.
   (Layout panel under Performance shows no layout thrashing on the parents.)

## Feature 2 — Bento ↔ Accordion with Context Lock
- One DOM. CSS renders a **bento grid** ≥769px and a **touch accordion** ≤768px.
- Accordion open/close uses the CSS `grid-template-rows: 0fr → 1fr` technique —
  smooth height animation, no JS measuring, no library.
- **Context Lock:** the active tile index is tracked. A `matchMedia` listener
  transfers it across the breakpoint — hover a tile on desktop, resize to mobile,
  and that exact panel is the one open. The choice also persists in
  `localStorage`.

## Motion (matches the brief)
- Micro-interactions (hovers/toggles): **180ms ease-out**
- Structural reflows (accordion): **350ms ease-in-out**
- Loader + entrance orchestration completes within **500ms** and never blocks TTI
  (content is in the DOM from first paint; the loader is a visual overlay only).

## SEO
Descriptive `<title>` + meta description, Open Graph + Twitter cards, canonical,
`theme-color`, `JSON-LD` (SoftwareApplication), `lang`, and accessible SVGs.

## Assets
Swap the placeholder palette + fonts in `styles.css` (`:root`) and the inline
SVGs in `index.html` with the files from `asset_package.zip`. See the README
files under `/assets`.

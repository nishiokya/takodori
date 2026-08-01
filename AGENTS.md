# Repository Guidelines

## Project Structure & Module Organization

This is a dependency-free static browser game.

- `index.html`: the complete application, including markup, CSS, game logic, inline SVG artwork, persistence, and sharing behavior.
- `README.md`: gameplay rules, feature documentation, and technical overview.
- `sitemap.xml`: the single canonical URL `https://takodori.tanishi.de/`.
- `og.svg` / `og.png`: Open Graph image source and render. Regenerate with `rsvg-convert -w 1200 -h 630 og.svg -o og.png`.
- `favicon.svg` / `favicon-32.png` / `apple-touch-icon.png`: site icons. The PNGs are rendered from `favicon.svg` with `rsvg-convert -w <size> -h <size> favicon.svg -o <file>`.

There is currently no separate asset directory, package manifest, build output, or automated test directory. Keep new code in `index.html` unless splitting files provides a clear maintenance benefit.

## Build, Test, and Development Commands

No build step is required.

```sh
open index.html
```

Opens the game directly. For more reliable URL-query and reload testing, serve it locally:

```sh
python3 -m http.server 8001
```

Then visit `http://localhost:8001/`. Use board URLs such as `/?b=k-8x14-14crtrn` to reproduce a specific layout.

```sh
git diff --check
```

Checks edited files for whitespace errors before committing.

## Coding Style & Naming Conventions

Use two-space indentation and preserve the existing plain HTML/CSS/JavaScript style. Do not introduce frameworks or dependencies for small changes.

- JavaScript functions and variables: `camelCase`, for example `computeSize`.
- DOM IDs and CSS classes: lowercase kebab-case, for example `btn-hint` and `stat-row`.
- Constants: uppercase snake case where already established, for example `THEME_DIMS`.
- Keep comments short and focused on non-obvious layout or game-rule decisions.

Maintain compatibility between the `tako` and `mahjong` themes; theme-specific styling should be explicitly conditional.

Do **not** list `?lang=` / `?theme=` / `?b=` URLs in `sitemap.xml`. `index.html` is a single file whose
`<link rel="canonical">` is hard-coded to `https://takodori.tanishi.de/`, so every parameter URL is
excluded by Google as "Alternate page with proper canonical tag" and only pollutes Search Console.
`sitemap.xml` stays at one entry. Per-language sitemap entries and `hreflang` alternates would only
become meaningful if the English version moved to its own path with its own canonical.

Keep the browser at the canonical top-level URL during ordinary play. Do not automatically add a
`?b=` board code with `history.replaceState`; generate board URLs only when the user explicitly
copies or shares one. This keeps the rendered URL stable for search engine indexing.

## Testing Guidelines

There is no automated test framework or coverage requirement. Perform browser smoke tests after every UI or game-logic change:

- Test `tako` and `mahjong`.
- Check mobile widths around 360px and 500px, plus desktop.
- Confirm no horizontal overflow, accidental wrapping, clipped controls, or console errors.
- Exercise tile matching, Hint, New, history, persistence, and shared board URLs when affected.

For smartphone immersive mode, keep all essential controls in the top HUD and minimize reserved
space around `#boardWrap`. Do not add a bottom control area that reduces tile size; verify the first
and last tile rows remain tappable around safe-area insets.

## Commit & Pull Request Guidelines

Use short, imperative commit subjects such as `Unify compact footer row` or `Flatten and widen tako layout`.

Pull requests should explain the user-visible change, why it is needed, and the validation performed. Include screenshots for visual changes when useful, especially mobile layouts. Keep each PR focused on one behavior and avoid unrelated cleanup.

## Security & Data

Game state is stored only in browser `localStorage`. Do not add secrets, analytics, or network transmission without documenting the behavior and obtaining explicit project approval.

# Repository Guidelines

## Project Structure & Module Organization

This is a dependency-free static browser game.

- `index.html`: the complete application, including markup, CSS, game logic, inline SVG artwork, persistence, and sharing behavior.
- `README.md`: gameplay rules, feature documentation, and technical overview.
- `sitemap.xml`: canonical language and theme URLs, including reciprocal `hreflang` alternates.

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

When adding a language or theme URL, update `sitemap.xml` with every supported language/theme combination. Escape query-string ampersands as `&amp;`, keep `hreflang` links reciprocal, and do not add seed-specific `?b=` board URLs.

## Testing Guidelines

There is no automated test framework or coverage requirement. Perform browser smoke tests after every UI or game-logic change:

- Test `tako` and `mahjong`.
- Check mobile widths around 360px and 500px, plus desktop.
- Confirm no horizontal overflow, accidental wrapping, clipped controls, or console errors.
- Exercise tile matching, Hint, New, history, persistence, and shared board URLs when affected.

## Commit & Pull Request Guidelines

Use short, imperative commit subjects such as `Unify compact footer row` or `Flatten and widen tako layout`.

Pull requests should explain the user-visible change, why it is needed, and the validation performed. Include screenshots for visual changes when useful, especially mobile layouts. Keep each PR focused on one behavior and avoid unrelated cleanup.

## Security & Data

Game state is stored only in browser `localStorage`. Do not add secrets, analytics, or network transmission without documenting the behavior and obtaining explicit project approval.

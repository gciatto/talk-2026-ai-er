# Shortcodes index

Custom shortcodes of this site. The reveal-hugo theme adds `section`, `slide`, `fragment`, `frag`, `note` (speaker notes), `math`, `mermaid` (`markdown` is overridden here).
Use `%` delimiters when the inner content is Markdown, `<` when it is HTML or no inner content.

<!-- No double curly braces in this file: Hugo parses everything under layouts/ as a template. -->

## Event-wide constants

| Shortcode | Renders |
|-----------|---------|
| `academic_year` | the period of the lectures (`Ottobre–Novembre 2026`) |
| `course_name` | the name of the course the lectures belong to |

## People

| Shortcode | Renders |
|-----------|---------|
| `gc` | name linked to email: Giovanni Ciatto |
| `gc-address` | the email address as a `mailto:` code link |

## Content inclusion

| Shortcode | Params | Purpose |
|-----------|--------|---------|
| `import` | `path` (from repo root) | Inlines a Markdown file (e.g. `reusable/*.md`) |
| `load` | `path`, `from`, `to` | Like `import`, restricted to a line range |
| `code` | `path` (from repo root), `from`, `to`, `language` (inferred from extension), `highlight` (default `true`) | Inlines a line range of a local file as a fenced code block; use with `%` |
| `github` | `owner`, `repo`, `branch`, `path`, `from`, `to`, `language` | Fetches a file from GitHub at build time and highlights a line range |
| `github-url` | `owner` (default `unibo-fc-isi-iag`), `repo` (default `slides-module2`), `branch`, `path` | URL of a repo, or of a file in it |
| `markdown` | inner | Renders inner content as-is (forces Markdown processing) |

## Layout

| Shortcode | Params | Purpose |
|-----------|--------|---------|
| `multicol` | `class` | Bootstrap row; wrap `col` shortcodes in it |
| `col` | `class`, `text-align` (default `left`) | One column inside `multicol` |
| `align-right` | `padding` | Right-aligned block |
| `small` | positional font size (default `60%`), inner Markdown | Shrinks the inner content (e.g. a wide table); use with `%` |
| `vspace` | positional height (default `20px`) | Vertical spacer |
| `image` | `src`, `alt`, `width`, `height`, `max-w` (default `95vw`), `max-h` (default `80vh`), `link` | Image scaled to fit the slide, optionally a link |

## Inline decorations

| Shortcode | Params | Purpose |
|-----------|--------|---------|
| `color` | positional CSS color, inner | Colored span |
| `comment_frag` | positional text, `class`, `index` | Grey text appearing as a fragment |
| `tick` / `cross` | — | Green check / red cross icon (FontAwesome) |
| `emoji` | positional emoji name | Emoji by name, e.g. `smile` |
| `today` | — | Build date (`YYYY-MM-DD`) |

## Diagrams and embeds

| Shortcode | Params | Purpose |
|-----------|--------|---------|
| `plantuml` | inner PlantUML source, `alt`, `width`, `height`, `max-w`, `max-h` | Renders PlantUML client-side (`static/plantuml.js`) |
| `gravizo` | `title`, `width` (or positional title), inner source | Graph via the Gravizo online service |
| `chart` | positional width % and height px, inner Chart.js config | Chart.js chart |
| `qrcode` | `data`, `width`, `height`, `image`, `dotsColor`, `dotsType`, `backgroundColor`, `margin`, `style` | Styled QR code |
| `mentimeter` | positional presentation id | Embedded Mentimeter poll (removed in print mode) |

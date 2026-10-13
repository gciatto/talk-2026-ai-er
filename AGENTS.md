# AGENTS.md

Guidance for AI agents (and humans) working on this repository.

## What this is

Slides of two lectures by Giovanni Ciatto, within the *AI Course — Regione Emilia-Romagna 2026 (Part 2)*
(other lectures of the course, on RAG, are given by other teachers):

1. `llmaas` — LLM-as-a-Service: APIs, tools, and agents (27/10/2026, 4h)
2. `governance` — Sovereign and open alternatives to large LLMs (18/11/2026, 3h)

Published at <https://gciatto.github.io/talk-2026-ai-er> (GitHub Pages, branch `gh-pages`, built by CI on every push to `master`).

### Audience

__Mixed__: managers and technicians, who already attended a lecture equivalent to `genai` of slides-module2 (don't repeat it). Hence:
- concepts, trade-offs, and decision criteria first; code only as _short excerpts_, plus the command to run it
- _guided_ exercises (live demos, or group work on paper), where participants change _one_ parameter; no exercises to solve at home,
  no solution walkthroughs, no "Check your understanding" decks
- no "Project Structure" slides; link the full file instead

## Where the contents come from

Most slides are __cherry-picked__ (copied, then trimmed) from [slides-module2](https://github.com/unibo-fc-isi-iag/slides-module2),
the slides of Module 2 of the University course *Intelligent Agents* (University of Bologna), by the same teacher.
[`SOURCES.md`](SOURCES.md) maps every slide to its origin (lecture and line upstream, or __new__).

- Lecture directories keep the names of the main upstream lecture they draw from (`llmaas`, `governance`), but mix slides of
  several upstream lectures: `llmaas` also takes from `prompting`, `agents`, `skills`, `workflows`, `governance`;
  `governance` also takes from `llmaas`, `agents`, `validating`, `free-access`
- Links of copied slides were rewritten:
    + to a slide present in _this_ repo $\rightarrow$ `#/anchor` (same deck) or `../<lecture>/#/anchor` (other deck)
    + to a slide __not__ copied $\rightarrow$ the published upstream slides, `https://unibo-fc-isi-iag.github.io/slides-module2/<lecture>/#/anchor`
- Pictures and data files used by copied slides were copied next to the deck using them
- Upstream fixes are __not__ propagated automatically: to port one, look the slide up in `SOURCES.md`, and diff it against the upstream
  heading (line numbers drift); conversely, generally useful improvements made here deserve to go upstream too
- New slides should follow the upstream structure (see below), and be added to `SOURCES.md` as __new__

## Code of the examples

All runnable code lives in [`lab-snippets`](https://github.com/unibo-fc-isi-iag/lab-snippets), mounted as the submodule `static/lab-snippets/`,
__shared with the University course__ (tracking its `master` branch, i.e. examples + solutions; participants clone `master`, not `exercises`):

- never edit it here: change it in a separate clone of lab-snippets (following _its_ `AGENTS.md`: smoke `CASES`, README order,
  merge `master` into `exercises`), push, then bump the submodule here (`git -C static/lab-snippets pull`, commit the pointer)
- after a bump, __re-check the `from`/`to` line ranges__ of every `code` shortcode pointing to changed files (here _and_ upstream)
- snippets are addressed by their upstream coordinates: `snippets/lecture_<NAME>/example<ID>/`, run as
  `poetry run python -m snippets -l <NAME> -e <ID>` (`-x <ID>` for exercise solutions, e.g. `-l governance -x 1`), from the clone's root
- LLMs are configured via `OPENAI_BASE_URL`, `OPENAI_API_KEY`, `OPENAI_MODEL` (defaults: Open Router, free models); keep examples runnable
  for free (Open Router `:free` models, or Ollama)
- don't run snippets inside `static/lab-snippets/` (it would create `.venv`/`__pycache__` there, which get published): use a separate clone

## Running locally

```bash
git submodule update --init --recursive   # theme, shared-slides, lab-snippets are submodules
hugo serve                                # requires Hugo *extended* (SCSS compilation)
```

Then open <http://localhost:1313/talk-2026-ai-er/>. `hugo` alone builds into `build/`. Printable version: append `?print-pdf` to a URL.

## Layout

| Path | Purpose |
|------|---------|
| `config.toml` | Hugo + reveal.js configuration (1920x1080, theme `league`, custom SCSS, mermaid) |
| `content/_index.md` | Landing presentation: ToC (`#/toc`), teacher, goals, prerequisites, how to set up and run the examples (`#/lab-snippets-setup`, `#/lab-snippets-run`) |
| `content/<lecture>/_index.md` | One lecture = one directory = one presentation, with its pictures and data files next to it |
| `layouts/shortcodes/` | Custom shortcodes (from upstream), documented in `layouts/shortcodes/INDEX.md`; event name and period in `course_name`, `academic_year` |
| `layouts/partials/reveal-hugo/{head,body}.html`, `assets/custom-theme.scss` | Extra CSS/JS and stylesheet (from upstream) |
| `reusable/` | Snippets imported into slides: `front.md`, `footer.md`, `back.md`, `running-example.md`, `antigravity.md` |
| `static/lab-snippets/` | Git submodule: the code of all examples (see above) |
| `themes/reveal-hugo/`, `shared-slides/` | Submodules (theme; build/serve/PDF scripts): do not edit |
| `SOURCES.md` | Provenance of every slide |
| `.github/workflows/build-and-deploy.yml` | CI: preprocess, `hugo`, inline mermaid, deploy to `gh-pages`, PDFs as release assets |

## Slide conventions (inherited from slides-module2)

- Lecture skeleton: front matter (`title = "[AI-ER] ..."`, `outputs = ["Reveal"]`), `# Title`, `{{% import path="reusable/footer.md" %}}`, an
  _Outline_ slide, the sections, a _Wrap-up_ slide, `{{% import path="reusable/back.md" %}}`; add each lecture (and its exercises) to the
  ToC in `content/_index.md`
- `---` separates slides; `{{% section %}}` ... `{{% /section %}}` groups vertical slides (one column per topic); `# ` = section title slide,
  `## ` = slide title; named anchors via `{{< slide id="..." >}}` (unique per deck: the ToC and the outlines link to them)
- Each topic follows the upstream _conceptual flow_: __general concept__ $\rightarrow$ __intuitive example__ (often a picture) $\rightarrow$
  __technological landscape__ (a comparison table, with links to docs) $\rightarrow$ __example__ (code) $\rightarrow$ __comparison__ /
  trade-offs $\rightarrow$ __exercise__; key take-aways in a final `>` quote, often as a `{{% fragment %}}`
- Emphasis: `_italic_` for key terms, `__bold__` for the central concept, often combined
- Examples: `## Example N: <Title> (pt. K)` slides, starting with a `> __Goal__:` line; code via
  `{{% code path="static/lab-snippets/snippets/.../file.py" from="10" to="20" %}}` (never pasted), full files linked as `../lab-snippets/...`,
  an _example of interaction_ (real output, as `text`), and what to observe when running it
- Guided exercises: `## Guided exercise N: <title>` (anchor `exercise-<name>`), with a `> __Goal__:` line, numbered steps, the commands,
  and the questions to discuss; group exercises say what to produce, and how to assess it
- Layout: `{{% multicol %}}{{% col %}}...{{% /col %}}{{% /multicol %}}`; wide tables in `{{% small "80%" %}}...{{% /small %}}`;
  images via `{{< image src="./pic.svg" max-h="60vh" alt="..." >}}` (always with a meaningful `alt`), never cropped: check them at 1920x1080
- Diagrams: mermaid code fences or `{{< mermaid >}}` (inlined by CI), or `{{< plantuml >}}`; `.graphml` sources exported to `.svg`/`.png`
- Facts that age (prices, leaderboards, model versions, licenses) carry their __date__ ("as of September 2026", "retrieved October 2026")
- European/Italian "sovereignty" is presented as _others'_ framing, stressing with evidence that EU/Italian models are lagging behind
- Slides are in __English__; `goldmark` runs in unsafe mode, so raw HTML is allowed
- Course-wide values (event name, period) live in single-line shortcodes in `layouts/shortcodes/`: never hard-code them in slides

## Don'ts

- Don't edit submodules in place; don't commit `build/`, `public/`, PDFs.
- Don't put any `index.md` / `INDEX.md` under `content/` (Hugo lowercases it, clashing with `_index.md`); any other `.md` there becomes a page.
- Don't write double curly braces in non-template files under `layouts/`: Hugo parses them as templates.
- Don't rename lecture directories, nor slide anchors linked from elsewhere: they are public URLs.

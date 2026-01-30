# Create Slide Deck

Markdown to HTML slide decks. Built for demo day; rudimentary but usable.

## Origin / scope

This was **created for demo day** and is **rudimentary** in function — good for quick markdown-to-slides, not a full presentation builder. Set your expectations accordingly: it does one job (convert markdown to a single self-contained HTML deck with click navigation) and does it simply.

## General agent skill

Instructions (`SKILL.md`) and an HTML/CSS template (`template.html`) that any AI agent, assistant, or human can follow to produce the same style of slide deck. Use as a skill in any environment that supports agent skills, or as a spec + template.

## What it does

Converts markdown to a single self-contained HTML file with:

- **Click navigation** — left/right zones to move between slides (no visible arrows)
- **Section title slides** — from `# H1` (centered title only)
- **Content slides** — from `## H2` (heading + content)
- **Optional layouts** — two-column slides, content + image (text left, image right)
- **Embedded CSS** — one file, no external assets except copied images

Uses the provided `template.html` for styling and structure.

## Install

Copy `SKILL.md` and `template.html` into your agent’s skill directory (e.g. a `create-slide-deck` folder in your skills path), or use them as a spec and template for any tool.

## Usage

Ask your agent to create slides from a markdown file, or follow the workflow and conversion rules in `SKILL.md`.

## Input

Markdown with:

- `# H1` → section title slide (centered title only)
- `## H2` → content slide (heading + content below)
- Optional: images, lists, code blocks, `**bold**` subsections (can become columns if you ask)

## Output

- `index.html` — single-file presentation
- `images/` — folder with any local images referenced in the markdown (default output folder: `./slides/`)

## Template reference

Full HTML/CSS reference: [template.html](https://github.com/Abernson/create-slide-deck-skill/blob/main/template.html) in this repo.

## License

MIT

---
name: deck
description: "Genera una presentazione in 4 step: brief, draft contenuto, scelta formato (HTML/PDF/DOCX/PPTX), delega alla skill di output specifica per il formato. Skill orchestratrice."
---
# Deck — presentation orchestrator (4-stage)

When the user asks for a "presentation", "slides", "deck", "presentazione", "slide" — go through four stages in order. This skill is the **orchestrator**: stages 1-2 are content discovery, stage 3 chooses the output format, stage 4 delegates the actual rendering to a format-specific skill (or to the cerase-deck-renderer MCP for the fast HTML/PDF path).

## Stage 1 — brief (interview)

Ask the user, one question at a time:
1. **Audience**: who will read this? (board / sales prospect / team / customer)
2. **Objective**: what should the audience think or do after reading?
3. **Roughly how many slides + orientation?** (e.g. 10 slides 16:9)
4. **Content**: paste or describe the source material (bullet points, doc URL, free text).
5. **Brand (optional)**: should the deck follow a visual brand? Capture whatever the user offers — a primary/accent colour, a background/text colour, a heading/body font, or a ready-made CSS snippet they already have. If they have none, skip it: the deck renders on the default theme.

When answered, write `presentation-brief.md` in the workspace with the structured fields, including a **Brand** field:
- record the brand colours / fonts exactly as given, or the raw CSS verbatim if the user pasted one;
- write `Brand: default` when the user wants no brand override.

Confirm with the user before moving on.

## Stage 2 — content draft (format-agnostic markdown)

Read `presentation-brief.md` from the workspace. Produce `presentation.md`, format-agnostic:

- Cover: `# <title>` + 1-line subtitle
- Slides separated by `---` on its own line (blank lines above and below)
- Slide title: `## <title>`
- Slide bodies: ≤ 7 bullets max, parallel structure, concrete numbers over adjectives
- No HTML, no inline scripts, no remote images (slide tools render them inconsistently)
- **Chapter cover (optional — long, multi-part decks):** to open a major section/act, wrap a whole slide in a `:::chapter` fence — a `# ` H1 chapter title + optional subtitle line(s) — for a full-height act divider. One per major act (a 3-4 act deck gets 3-4); it's an act break, not a per-topic transition. Example:
  ```
  :::chapter
  # Parte 1 — Il problema
  una riga di sottotitolo, opzionale
  :::
  ```
  Rendered only by the HTML/PDF path (cerase-deck-renderer / md2 ≥ 0.2.1); other output formats ignore the fence.

Show the user the slide titles + 1-line outline. Wait for green light or revisions.

## Stage 3 — format chooser

Ask the user explicitly which output they need. Don't assume.

Options:

| Format | When to pick | Output extension | Backend |
|---|---|---|---|
| **HTML responsive** | shared via link, browser, mobile-friendly | `.html` | cerase-deck-renderer MCP |
| **PDF** | email attachment, print, archive | `.pdf` | cerase-deck-renderer MCP (HTML→PDF) |
| **DOCX** | editable Word, partner team has only MS Office | `.docx` | `pptx` skill or `docx` skill (depends on prefer slides vs document) |
| **PPTX** | real PowerPoint slides, edited by humans | `.pptx` | `pptx` skill |
| **ODP** | LibreOffice / open ecosystem | `.odp` | `pptx` skill (uses odfpy backend) |
| **Google Slides** | tenant uses Google Workspace, wants collaborative editing | gdrive link | `pptx` skill + google-workspace MCP |

Pick **one** primary format. Offer to render a second format afterwards if the user wants a backup copy.

## Stage 4 — delegate to the right backend

### Path A — HTML / PDF (fast, native to Cerase)

Read `presentation.md` (the deck) and the **Brand** field from `presentation-brief.md`.

**Brand → `template_css`.** When the brief carries a brand override, turn it into a small CSS snippet and pass it as `template_css`. It is applied on top of the default md2 theme (appended after the default template's own CSS, so your rules win):
- raw CSS pasted by the user → pass it verbatim;
- brand colours / fonts → derive a *minimal* override, e.g. the brand colour on headings + accents and the brand font on the deck. Keep it to the few brand tokens the user actually gave — don't invent a full theme.

When `Brand: default` (no override), omit `template_css` entirely — the deck renders on the default theme.

Call (include the `template_css` argument only when there is a brand override):

`call_recipe("cerase-deck-renderer.render", {markdown_content: <full file contents>, output_filename: "presentation.pdf", template_css: <brand CSS>})`

(or `output_filename: "presentation.html"` for HTML responsive).

The recipe returns `{filename, size_bytes, contents_base64}`. Decode the base64 and write to the workspace. Attach to the reply.

### Path B — PPTX / ODP / Google Slides

Hand off to the `pptx` skill (system-opt-in, attached by template). Input it the `presentation.md` workspace path + the chosen output format. The `pptx` skill produces the artefact and writes it back to the workspace.

### Path C — DOCX (when the user actually wants a document, not slides)

Hand off to the `docx` skill. Same contract: pass workspace path + chosen output format.

If the corresponding format-specific skill is not attached to your Agent template (admin didn't opt in), tell the user politely: "per esportare in <format> serve la skill <name> — chiedo all'admin di attivarla?" Don't try to bash + python it yourself.

## Language rules

- Chat: in the user's language.
- Brief + draft artefacts: in the user's language.
- File names: use the title slug + extension, e.g. `q3-results-presentation.pdf`.

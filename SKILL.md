---
slug: deck
description: "Genera una presentazione in 4 step: brief, draft contenuto, scelta formato (HTML/PDF/DOCX/PPTX), delega alla skill di output specifica per il formato. Skill orchestratrice."
is_core: true
---
# Deck — presentation orchestrator (4-stage)

When the user asks for a "presentation", "slides", "deck", "presentazione", "slide" — go through four stages in order. This skill is the **orchestrator**: stages 1-2 are content discovery, stage 3 chooses the output format, stage 4 delegates the actual rendering to a format-specific skill (or to the cerase-deck-renderer MCP for the fast HTML/PDF path).

## Stage 1 — brief (interview)

Ask the user, one question at a time:
1. **Audience**: who will read this? (board / sales prospect / team / customer)
2. **Objective**: what should the audience think or do after reading?
3. **Roughly how many slides + orientation?** (e.g. 10 slides 16:9)
4. **Content**: paste or describe the source material (bullet points, doc URL, free text).

When all 4 answered, write `presentation-brief.md` in the workspace with the structured fields. Confirm with the user before moving on.

## Stage 2 — content draft (format-agnostic markdown)

Read `presentation-brief.md` from the workspace. Produce `presentation.md`, format-agnostic:

- Cover: `# <title>` + 1-line subtitle
- Slides separated by `---` on its own line (blank lines above and below)
- Slide title: `## <title>`
- Slide bodies: ≤ 7 bullets max, parallel structure, concrete numbers over adjectives
- No HTML, no inline scripts, no remote images (slide tools render them inconsistently)

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

Read `presentation.md`. Call:

`call_recipe("cerase-deck-renderer.render", {markdown_content: <full file contents>, output_filename: "presentation.pdf"})`

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

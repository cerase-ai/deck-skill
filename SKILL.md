---
name: deck
description: "Builds a presentation in five steps: a brief, a draft, a first render, a revision of the printed deck against eight checks, and the final render. Delivers it as HTML or PDF, or through the pptx or docx skill as PowerPoint, OpenDocument, Google Slides or Word. Use when the person asks for a presentation, slides, a deck or a pitch."
---
# Deck — a presentation in five steps

Each step writes a file in the workspace that the next one reads. Go through them in order, and tell the person in one line which step you are on.

| Step | Read first | Reads | Writes |
|---|---|---|---|
| 1. Brief | `brief.md` | the interview | `presentation-brief.md` |
| 2. Draft | `draft.md` | `presentation-brief.md` | `presentation.md` |
| 3. Format and first render | this file | `presentation.md` | `outputs/presentation.pdf` |
| 4. Revision | `revise.md` | the rendered PDF, `presentation.md`, the brief's Audience block | `presentation-revision.md`, and `presentation.md` corrected |
| 5. Final render | this file | `presentation.md` | the file in the format the person chose |

`brief.md`, `draft.md` and `revise.md` sit next to this file. Read each one when its step starts, not before. `draft.md` names four more files — `slide-patterns.md`, `copy-rules.md`, `md2-syntax.md`, `print-constraints.md` — and they are read only while drafting.

When a step's input file is missing, do not invent it: offer to run the step before, or ask the person to paste the content.

**A deck is not delivered before step 4 has run.** The author of a deck reads back into each line what they meant, so a deck checked only by its author still carries the lines its reader cannot parse. `revise.md` says why and how.

## Step 3 — format, then the first render

Ask which format the person needs, and do not assume one:

| Format | When | Extension | Made by |
|---|---|---|---|
| **HTML** | shared by link, read in a browser or on a phone | `.html` | the deck renderer |
| **PDF** | attached to a mail, printed, archived | `.pdf` | the deck renderer |
| **PPTX** | real PowerPoint slides that people will edit | `.pptx` | the `pptx` skill |
| **ODP** | LibreOffice | `.odp` | the `pptx` skill |
| **Google Slides** | the organisation works in Google Workspace and wants to edit together | a Drive link | the `pptx` skill |
| **DOCX** | the person wants a document rather than slides | `.docx` | the `docx` skill |

Whatever the format, the first render is a PDF: the revision reads the deck as it prints.

```
call_recipe("cerase-deck-renderer.render", {"markdown_content": "<the whole of presentation.md>", "output_filename": "presentation.pdf", "orientation": "landscape", "paper": "A4"})
```

`orientation` and `paper` come from the brief's Format section. It answers `{path, filename, size_bytes, format, pages}`: the PDF is in your workspace at `path`, and `pages` is how many pages it printed. A deck prints one page for the cover and one per slide, so count the lines that hold only `---` in `presentation.md`, add one, and compare: more pages than that means a slide ran onto a second page, which step 4 fixes.

When the renderer refuses the markdown, its message names the problem; fix the block it names, using `md2-syntax.md`, and render again, at most twice. After two failures, show the person the message and ask which block to drop.

### Brand

When the brief records brand colours, fonts or CSS, turn them into a short CSS snippet and add `"template_css": "<css>"` to every render call. It is applied on top of the default theme, so your rules win:
- CSS the person pasted: pass it as it is;
- colours or fonts: write a minimal override, such as the brand colour on headings and the brand font on the deck, with only the values the person gave.

When the brief says `Brand: default`, leave `template_css` out. A palette name (`palette = "cool"`) goes in the deck's frontmatter instead, as `md2-syntax.md` shows.

## Step 5 — the final render

After the revision has corrected `presentation.md`:

- **HTML**: `call_recipe("cerase-deck-renderer.render", {"markdown_content": "<presentation.md>", "output_filename": "<slug>.html", "orientation": "landscape", "paper": "A4"})` returns the HTML deck, one file that opens in any browser.
- **PDF**: the same call with `"output_filename": "<slug>.pdf"`. Compare `pages` with the slide count again.
- **PPTX, ODP, Google Slides**: hand over to the `pptx` skill with the path `presentation.md` and the format.
- **DOCX**: hand over to the `docx` skill with the path `presentation.md` and the format.

If the skill a format needs is not among your skills, say in the person's language that this format needs that skill, which the organisation's admin enables, and offer HTML or PDF. Do not build the file with bash yourself.

Deliver with `[[attach: <path>]]`. Never paste a file's content or any base64 in the chat. The file name is the deck's title as a slug, such as `q3-results-northwind.pdf`.

These are the renderer calls this skill makes. Do not invent others.

## Language

- Chat: the person's language, always.
- Brief, deck and revision: the deck's language, which the brief records; by default the person's language.

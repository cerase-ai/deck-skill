# deck-skill

A Cerase skill that has the assistant build a presentation in five steps: a
brief, a draft, a first render, a revision of the printed deck against eight
checks, and a final render. The assistant uses it when someone asks for a
presentation, slides, a deck or a pitch.

## What the assistant does

1. **Brief** (`brief.md`). Interviews the person one or two questions at a
   time: the audience, including the terms it uses every day and the ones it
   would have to look up; the objective and the exact ask; whether the deck is
   presented, read alone or both; orientation and paper; the length; the
   brand; the numbers, claims, sources, quotes and sections the deck must
   carry; the tone. Checks that the audience is concrete, the objective single
   and at least one fact provided, then writes `presentation-brief.md`.
2. **Draft** (`draft.md`). Fills the brief's gaps with targeted questions,
   proposes a narrative arc chosen from the objective (pyramid, SCQA, three
   acts, or problem-solution-application) as one sentence per slide, maps each
   slide to a pattern, and writes `presentation.md` in md2 markdown. While
   drafting it reads four reference files: `slide-patterns.md` (fourteen slide
   types with md2 blocks), `copy-rules.md` (seven procedures and twelve writing
   rules: takeaway titles, no rhetorical constructions, the label and decode
   tests, provenance, sources as links), `md2-syntax.md` (the syntax of md2
   0.2.1, the version the renderer runs) and `print-constraints.md` (what fits
   on a printed page, with line ceilings measured on the renderer).
3. **Format and first render.** Asks for the format (HTML, PDF, PPTX, ODP,
   Google Slides or DOCX), then renders a PDF with
   `cerase-deck-renderer.render`, passing orientation, paper and the brand as
   `template_css`, and compares the page count the renderer returns with the
   number of slides.
4. **Revision** (`revise.md`). A sub-agent started with only the rendered PDF,
   the markdown and the brief's Audience block runs eight checks: the
   takeaway, the delete test, the label test, the decode test, the page
   budget and lines the renderer broke, provenance with every link opened, the
   actor, and the cover. It writes `presentation-revision.md` and corrects
   `presentation.md`; without a sub-agent the assistant runs the checks itself
   and says so. The assistant reports every finding to the person.
5. **Final render.** HTML or PDF through the renderer; PPTX, ODP and Google
   Slides through the `pptx` skill; DOCX through the `docx` skill.

The renderer writes the deck to `outputs/` in the workspace and returns its
path, and the assistant attaches it with `[[attach: <path>]]`. Chat follows
the person's language; the brief, the deck and the revision follow the deck's
language, by default the person's.

## Requirements

- The `cerase-deck-renderer` connector for HTML and PDF.
- The `cerase-docreader` connector, which the revision uses to read the PDF.
- The `pptx` and `docx` skills for the other formats.

## Files

| File | Purpose |
|---|---|
| `SKILL.md` | The five steps, the render calls, the format table and the brand override. |
| `brief.md` | Step 1: the interview, the checks before writing, and the brief's template. |
| `draft.md` | Step 2: the arc, the pattern mapping, the writing checklist and md2 pitfalls. |
| `slide-patterns.md` | Fourteen slide patterns with md2 blocks. |
| `copy-rules.md` | The writing procedures and rules. |
| `md2-syntax.md` | md2 0.2.1 syntax: frontmatter, charts, columns, chapter covers. |
| `print-constraints.md` | What fits on a printed page, and the line ceilings. |
| `revise.md` | Step 4: who runs the checks, the eight checks and the revision file. |
| `cerase.json` | Marketplace manifest: namespace `studio.guidance`, name `deck`, display name, description, licence. |
| `i18n.yaml` | Italian display name and description for the Marketplace; not sent to the assistant. |
| `LICENSE` | MIT licence text. |

## Installation

Published in the Cerase Marketplace as `studio.guidance/deck`
([marketplace page](https://marketplace.cerase.ai/en/p/studio.guidance/deck)).
A Cerase appliance also ships it in its image and attaches it to every
assistant; an administrator cannot detach it.

## License

MIT. See [LICENSE](LICENSE).

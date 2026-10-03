# deck-skill

A Cerase skill that has the assistant build a presentation in four stages:
a brief, a markdown draft, a choice of output format, and a hand-off to the
tool or skill that produces that format. The assistant uses it when someone
asks for a presentation, slides or a deck.

## What the assistant does

1. **Brief.** Asks, one question at a time, for the audience, the objective,
   the number of slides and orientation, the source content, and an optional
   brand (colours, fonts or ready-made CSS). Writes `presentation-brief.md` in
   the workspace, with `Brand: default` when there is no brand, and confirms it
   with the person.
2. **Draft.** Writes `presentation.md`: a `#` cover with a one-line subtitle,
   slides separated by `---`, a `##` title per slide, at most seven bullets per
   slide, no HTML and no remote images. Long decks may open each act with a
   `:::chapter` block, which only the HTML/PDF path renders. Shows the slide
   titles and waits for approval.
3. **Format.** Asks which output is needed (HTML, PDF, DOCX, PPTX, ODP or
   Google Slides), produces one, and offers a second format afterwards.
4. **Hand-off.**
   - HTML or PDF: calls `cerase-deck-renderer.render` with the markdown, and
     passes the brand as `template_css` when there is one.
   - PPTX, ODP or Google Slides: hands `presentation.md` to the `pptx` skill.
   - DOCX: hands it to the `docx` skill.
   - When the needed skill is not attached to the assistant, says that an
     administrator has to enable it, instead of producing the file another way.

The renderer always produces a PDF, so asking it for `presentation.html`
returns PDF content under that name.

Chat and documents follow the person's language; file names are the title as a
slug plus the extension, for example `q3-results-presentation.pdf`.

## Requirements

- The `cerase-deck-renderer` connector for HTML and PDF.
- The `pptx` and `docx` skills for the other formats.

## Files

| File | Purpose |
|---|---|
| `SKILL.md` | The instructions the assistant loads: `name` and `description` frontmatter, then the four stages. |
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

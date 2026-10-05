# Print constraints

The rules that keep a deck correct when it prints. Each one prevents a failure that happens in print and not on screen. When in doubt, follow them: they put a correct page before a dense one.

The render answer carries `pages`. A deck prints one page for the cover and one per slide, so the expected count is the number of lines holding only `---`, plus one. Any page more is a slide that ran over.

## 1. One chart per slide

At most one `:::chart` per slide. A chart is never split across pages: when a chart and other heavy content do not fit on one page, the chart moves whole to the next page, which then has a chart and no title, and the page before is half empty.

**Fix**: two slides. The first sets the context, the second carries the chart with one caption line.

## 2. Beside a chart, one or two short lines

Next to a chart, only the `## ` title and one or two short sentences, about 30 to 50 words together. A chart's height follows the page, so more text pushes it to the next page (rule 1).

**Fix**: move the explanation to the slide before or after, as a text slide.

## 3. A pie stands almost alone

A slide with a pie holds its title and at most one short line: no quote, nothing else. A pie with its legend takes about half of the page by itself.

**Fix**: move the description to the slide before.

## 4. Bars and columns: the largest at most ten times the smallest

Within one bar or column chart, keep the largest value at most about ten times the smallest. The scale is linear: at a ratio of 140 the small bar is thinner than its label, which then breaks one character per line.

**Fix**: split the chart into large and small values; or show shares of a total; or move the smallest values into the caption; or drop a value that does not carry the message.

## 5. A table can carry more than a chart

A table slide may have one or two sentences above and a short `> ` takeaway below. A table breaks across pages cleanly and keeps its header row on each page.

**Width**: a table too wide for the page does not scroll in print, it is cut on the right. Drop a column, split it over two slides, or turn it into a list.

## 6. No sparse slides, and no slide with only a title

Every slide fills between a third and two thirds of the page. A slide with one short bullet and the rest blank reads as a placeholder.

**Fix**: merge it with its neighbour on the same subject; make it a hero number or a quote, which use white space on purpose; or cut it.

Every slide carries something under its title, section dividers included: a line saying what comes next. Two or three dividers per deck at most.

## 7. Every slide has a `## ` title

A slide without one is listed as "Slide N" and prints with no title. If you cannot find its title, the slide probably should not exist (rule 6).

## 8. Two-column slides stay light

On a `:::columns` slide: at most one short line above the columns, no quote below, about four short bullets per column, and the two columns of similar length. The columns block never splits across pages, so a heavy one moves whole to the next page and leaves the title alone on the page before.

**Fix**: one line above at most; remove whole bullets, never shorten them (`copy-rules.md` §0 c); if both columns are heavy, two slides.

## 9. Read the PDF, then adjust

After every render, compare `pages` with the expected count, and read the PDF's text for:
- pages with an empty lower half (rule 6): merge or fill;
- a chart on a page without its title (rules 1 to 3): **remove** text beside it, a whole sentence or bullet, rather than shortening it;
- labels cut on small bars (rule 4): split the chart or drop the smallest values;
- lines the renderer broke (rule 10).

## 10. No line broken by the renderer

Every printed line is a line the author wrote. Where the renderer would wrap, the line is shortened or broken by hand at a phrase boundary; a break the renderer chooses lands mid-phrase and shows where the text ran out of room, not where the author wanted it to stop. This covers slide titles, chapter titles and subtitles, the cover, body text, bullets and quotes. In a table, headers and first-column labels stay on one line; long text inside a cell may wrap, because a cell cannot choose its breaks.

**Line ceilings** on A4 landscape with the default template, measured on the renderer. Characters vary in width, so treat them as ceilings and check after the render:

| Text | Fits | Write at most |
|---|---|---|
| Cover title, the frontmatter `title` | about 45 characters | 40 |
| Chapter title | about 34 characters | 30 |
| Chapter subtitle | about 93 | 85 per line |
| Slide `## ` title | about 94 | 85 |
| Body, bullets, quotes | about 119 | 110 per line |
| Cover subtitle | about 145 | 135 per line |
| Table header, row label | the column's width | never wraps: shorten the label |

On portrait pages each ceiling is about seven tenths of these.

**Fix**: break the source line by hand at a phrase boundary, or remove words; the line is too long, not the font too large. Never accept the renderer's break.

**Check after the render**: read the PDF's text with `call_recipe("cerase-docreader.read_document", {path: "outputs/presentation.pdf"})` and compare its lines with the lines of `presentation.md`. A printed line that is the beginning of a source line, with the rest on the next printed line, is a wrap. Headers inside table cells need an eye of their own: the comparison does not see inside cells.

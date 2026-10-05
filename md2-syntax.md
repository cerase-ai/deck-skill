# md2 syntax

The markdown the deck renderer reads: md2 0.2.1, the version the renderer runs. Everything the draft needs is here.

> **The frontmatter is `+++` (TOML), never `---`.** In md2 `---` separates slides, so a frontmatter written between `---` lines becomes a slide and the rendering breaks without an error.
>
> **The frontmatter starts on line 1.** Anything above `+++`, a blank line included, makes md2 print the raw block on the cover and title the deck "Presentation".

## The file

Three parts:

1. **Frontmatter**: TOML between `+++` fences.
2. **Cover**: everything before the first `---`, a `# ` title and a paragraph. The cover prints the frontmatter `title` when there is one, and the `# ` line only when there is not, so write the same title in both.
3. **Slides**: separated by `---` on a line of its own, with a blank line above and below.

```markdown
+++
title = "Q3 results for the Northwind board"
palette = "cool"
lang = "en"
+++

# Q3 results for the Northwind board

Revenue, costs and the hire we ask the board to approve.

---

## Revenue grew 18% on the quarter

Body of the first slide.

---

## The hire costs €58,000 a year

Body of the second slide.
```

## Frontmatter

| Field | Type | Default | What it does |
|---|---|---|---|
| `title` | string | the cover's `# ` title | the title the cover prints, and the page title; when it is set, the cover's `# ` line is not printed |
| `palette` | string | `"default"` | one of `default`, `warm`, `cool`, `mono`, `vivid`, `pastel` |
| `colors` | array of hex strings | — | replaces the palette's first colours, in order: `colors = ["#1F4E79", "#E07A1F"]` |
| `lang` | string | `"it"` | the page's language; set it to the deck's language |
| `dark` | bool | `false` | opens on the dark theme |

Only these six palettes exist on the renderer. Brand colours go in `colors`, or in the brand CSS the render call takes.

## Headings

| Level | Where | What it does |
|---|---|---|
| `# ` | the cover, a chapter cover, a hero number | the deck's title; inside a slide, a very large line |
| `## ` | one per slide | the slide's title, also its entry in the side navigation |
| `### ` | inside a slide | a sub-heading |

A slide with no `## ` title is listed as "Slide N". Give every slide one.

## Charts — `:::chart`

A markdown table drawn as a chart. The first column holds the labels, every other column is a series.

```
:::chart column --title "Orders per quarter"
| Quarter | Orders |
|---------|--------|
| Q1      | 1200   |
| Q2      | 1310   |
| Q3      | 1450   |
:::
```

| Type | For |
|---|---|
| `bar` | horizontal bars: a ranking |
| `column` | vertical bars: values over time |
| `stacked-bar`, `stacked-column` | the same, with the series stacked; negative values fall back to side by side |
| `line` | a trend |
| `line filled` or `area` | a trend with the area under it filled |
| `pie` | shares of one whole, one series only |

The only option is `--title "…"`, the caption above the chart; a `# ` heading inside the block does the same. Labels are always shown. Values are printed on bars and columns, and a pie lists them in its legend. A chart with two or more series gets a legend by itself. Line and area charts show a scale and no values.

A chart with two series:

```
:::chart column --title "Revenue and costs, € thousand"
| Quarter | Revenue | Costs |
|---------|---------|-------|
| Q1      | 410     | 350   |
| Q2      | 455     | 362   |
| Q3      | 498     | 371   |
:::
```

How large each type prints is in `print-constraints.md`.

## Two columns — `:::columns`

Two `:::col` parts inside a `:::columns` block, two columns at most. On a phone they stack; in print they stay side by side.

```markdown
:::columns

:::col
**Today**
- Orders keyed by hand: 6 hours a week
- Stock checked by phone

:::col
**From March**
- Orders read from the mail: 1 hour a week
- Stock visible to every branch

:::
```

## Chapter cover — `:::chapter`

A whole slide turned into the opening of a part of the deck: full height, the title large, an optional line under it.

```
:::chapter
# Where the money goes
The three cost lines that grew, and why.
:::
```

The `# ` line is the chapter's title and its navigation entry; whatever follows it is the subtitle. Only the `:::chapter` fence makes a chapter: a `# ` line inside an ordinary slide stays a large heading. Like any slide, the fence has a `---` before and after it, with blank lines.

## Everything else

- `**bold**`, `*italic*`, `` `code` ``.
- Links: `[label](https://…)`; a bare URL is linked too, but a link written on a name is the one a reader of the PDF can click.
- Images: `![description](https://…)`, centred; `<img src="https://…" width="200">` sets a size. The image must be reachable by https: the renderer receives only the markdown, so a file in the workspace is not found.
- Lists with `-`, `*` or `1.`, nested by indentation.
- Tables with `|` and a `---` separator row; `:---`, `:---:` and `---:` align a column.
- `> ` makes a quote with a coloured bar.
- Footnotes: `[^1]` in the text and `[^1]: …` at the bottom of the slide.
- Code blocks between three backticks.
- A single line break is kept as a line break.

`<iframe>` and `<img>` are allowed; `<script>`, `onclick` and `javascript:` are removed.

## Keys in the HTML deck

Worth telling the person when you send the HTML file:

| Key | Does |
|---|---|
| `↓` `→` `PgDn` | next slide |
| `↑` `←` `PgUp` | previous slide |
| `Home` / `End` | cover / last slide |
| `S` | shows or hides the side navigation |
| `D` | switches dark and light |
| `Ctrl+P` | prints a clean PDF |

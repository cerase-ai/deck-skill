# Step 2 — the draft

Read `presentation-brief.md`, fill what it is missing, agree a narrative arc with the person, map each beat to a slide pattern, and write `presentation.md` in the workspace, in md2 markdown ready for the renderer.

If `presentation-brief.md` is missing, offer to run the brief first, or ask the person to paste it. If `presentation.md` already exists, ask whether to replace it or write under another name.

## The four reference files

They sit next to this file. Read each one when you reach the work it covers, and not before:

| File | Read it when |
|---|---|
| `slide-patterns.md` | choosing the pattern for a beat, or writing a slide |
| `copy-rules.md` | writing or rewriting any line: titles, bullets, captions |
| `md2-syntax.md` | writing the frontmatter, a chart, columns, a chapter cover |
| `print-constraints.md` | sizing a slide: a chart, a pie, a table, two columns |

They are the rules for their subject. Apply them as written; do not paraphrase them from memory.

## Procedure

### 1. Read the brief

- Audience: the tone, the density, what can stay implicit, and the vocabulary every term is tested against.
- Objective: the narrative arc.
- Format: the density per slide.
- Length: the slide count.
- Brand: the palette and the language.
- Content: what must appear.
- Tone: the words and the sentence shape.

### 2. Fill the gaps

Compare the brief with what the deck needs. The usual gaps: a number with no source; a claim with no data; a decide or approve objective with no stated ask; a visual named but not provided. Ask two or three targeted questions for those gaps only, never the whole interview again. If the person says to go ahead without, write that down and go ahead.

### 3. Propose the arc

Choose the framework from the objective:

| Objective | Framework |
|---|---|
| Decide, approve | **Pyramid**: the conclusion first, then three groups that support it |
| Persuade | **SCQA**: situation, complication, question, answer |
| Update, inform | **Three acts**: where we were, what changed, what comes next |
| Teach | **Problem, solution, application** |

Show the person the arc as one line per slide, each line the sentence the slide establishes, written out in full (`copy-rules.md` rules 1 and 7b). **No sentence, no slide** (`copy-rules.md` §0 a): a slide whose sentence you cannot write at this point has no takeaway, so merge it into its neighbour or cut it now. Wait for the person's agreement or changes.

### 4. Map each beat to a pattern

Pick a pattern from `slide-patterns.md` for each line of the arc; its last table is the starting guide. Then check:
- the cover (pattern 1) is slide 1, and the closing ask (pattern 13) is the last slide;
- section dividers (pattern 2) appear two or three times at most, only between major parts, and each carries a line under its title;
- a deck in several acts opens each act with a chapter cover (pattern 2b), one per act;
- hero numbers (pattern 3) appear once or twice, on the strongest numbers.

Two neighbouring beats that want the same pattern may be one slide.

### 5. Write `presentation.md`

- **Frontmatter**: the `+++` block is the very first line of the file, with `title`, `lang` and `palette` or `colors` from the brief (`md2-syntax.md`).
- **Cover**: pattern 1, a title that names the subject (`copy-rules.md` rule 10). The cover prints the frontmatter `title`, so the `# ` line repeats it word for word.
- **Slide titles**: every `## ` title states the slide's takeaway as a plain declarative sentence (`copy-rules.md` rule 1). Test each twice: could it sit unchanged on another deck, then it is a label; does the reader learn a fact from it or only feel confidence, then it is a slogan. Rewrite either way.
- **No rhetorical constructions** anywhere, cover and subtitles included (`copy-rules.md` rule 7b). This holds for every audience, a board included.
- **Bullets**: six at most; about six words each when presented, ten to twelve when read alone (`copy-rules.md` rule 6).
- **Numbers**: every number the argument rests on names its source on the slide (`copy-rules.md` rule 8).
- **No closing line by default** (`copy-rules.md` §0 b): a slide may end on its table, its chart or its facts.
- **Every label passes the label test** (`copy-rules.md` §0 d): every column header and every first-column row label, read with one value under it, says what the value asserts. In a deck written in another language than its sources, derive each label again from its values rather than translating it.
- **Chart slides**: one or two short lines beside the chart, no second chart and no table on the same slide; a pie stands alone (`print-constraints.md` rules 1 to 3).
- **Bars and columns**: the largest value at most ten times the smallest, or split the chart (`print-constraints.md` rule 4).
- **No sparse slides and no title-only slides**: every slide fills at least a third of the page and carries a line beyond its title (`print-constraints.md` rule 6).
- **Every slide has its `## ` title** (`print-constraints.md` rule 7); a chapter cover has its `# ` title inside the fence.

### 6. Check before declaring the draft done

- [ ] The cover names the subject, with one line saying what the deck is for.
- [ ] Every slide has a `## ` title stating a takeaway as a plain sentence.
- [ ] No rhetorical constructions: triads, antithesis, chiasmus, fragments for emphasis, aphorisms, wordplay. Read every chapter and section subtitle again: that is where they hide.
- [ ] No slide carries only its title.
- [ ] No slide has more than six bullets.
- [ ] Every chart slide has at most two short lines beside the chart, and every pie slide only its title and one line.
- [ ] No bar or column chart has a ratio above ten between its values.
- [ ] Every number the argument rests on names its source.
- [ ] No filler phrases (`copy-rules.md` rule 7).
- [ ] The label test passes on every column header and every first-column row label, on every slide.
- [ ] No slide ends on a line that exists because the layout seemed to want one.
- [ ] Nothing was made to fit by shortening a sentence (`copy-rules.md` §0 c): a fact, a row or a slide came out instead, and no definition was among them.
- [ ] The decode test (`copy-rules.md` §0 e): every method, standard, rule, material, instrument, unit and acronym carries its meaning the first time it appears, tested against the brief's vocabulary lists and not against your own.
- [ ] The last slide states the ask.

Fix whatever fails, then hand back to step 3 of `SKILL.md`: the first render checks that the markdown parses and that every slide fits its page.

## md2 pitfalls

The usual reasons the renderer refuses a deck or renders it wrong:

- **Frontmatter fence**: md2 uses `+++` (TOML). `---` is the slide separator, so a frontmatter written between `---` lines becomes a slide and the real first slide disappears.
- **The frontmatter starts on line 1**: anything above `+++`, a blank line included, makes md2 print the block on the cover and title the deck "Presentation".
- **A separator needs a blank line above and below**, or md2 keeps appending to the current slide and prints `---` as text. Twelve slides written and seven rendered is this.
- **`:::chart` and `:::columns` blocks need a blank line above and below**, and the closing `:::` on its own line.
- **A chart's table needs a header row, a separator row and at least two data rows**; a single row renders an empty box.
- **Pie values are positive numbers.**
- **Every table row has as many `|` cells as the header**; one short row turns the whole block into plain text.
- **No chart inside `:::columns`**: put the chart on its own slide.
- **Column slides overflow in print** when an intro paragraph and a closing quote surround them; keep them light (`print-constraints.md` rule 8).

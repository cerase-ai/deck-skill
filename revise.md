# Step 4 — the revision

Read the rendered deck as its audience will, run eight checks on it, write what they find in `presentation-revision.md`, correct `presentation.md`, and render again. A deck is not delivered before this step has run.

## Why the author cannot do this step

The author knows what each line meant, so when re-reading they put the meaning back into the line whether or not the words carry it. A reader sees in seconds a line the author re-read three times without seeing, and a line the author rewrites right after being told can be unreadable again, in the same hour, with the rule in hand. Re-reading your own draft is not a check.

## Inputs

| What | Where | Used for |
|---|---|---|
| The rendered deck | `outputs/presentation.pdf`, and the `pages` the render answered | the printed order, the page budget, the lines as they print |
| The deck's source | `presentation.md` | the copy under review, and where the fixes go |
| The audience | the `## Audience` block of `presentation-brief.md`, **that block only** | who reads the deck, and which words they have |

Without the PDF, go back to the render: checks (c) and (e) have nothing to work on. Without the brief, say so in the revision file and run check (d) against an audience you state, not silently against your own vocabulary.

## Who runs the checks — read this first

**Whoever runs the checks gets the deck and not the reasoning behind it**: no brief beyond the Audience block, no research notes, no source material, no chat, no "this slide is there because".

Run them through a sub-agent: start one with your task tool, of type `general`, and give it exactly this:
- the paths `outputs/presentation.pdf` and `presentation.md`, and the `pages` value of the last render;
- the text of the brief's Audience block;
- this file's path, with the instruction to run the eight checks below, write `presentation-revision.md` in the format below, and apply the fixes to `presentation.md`.

It reads the PDF's text with `call_recipe("cerase-docreader.read_document", {path: "outputs/presentation.pdf"})`.

If you cannot start a sub-agent, run the checks yourself and write in the revision file that the drafting assistant ran them: the pass is weaker by exactly what you remember, and a pass by the author that finds nothing does not show the deck is clean.

The Audience block is not reasoning: it says who reads and which words they know, nothing about why a slide exists.

## The eight checks

Run all eight, slide by slide, in printed order.

### (a) The takeaway

For each slide, write the one sentence it establishes.
- **Cannot be written**: the slide has no takeaway; merge it into its neighbour or cut it.
- **Can be written but is not on the slide**: put it on the slide, as its title.
- **Written and already there**: next slide.

### (b) The delete test

For every line that is not a fact, delete it and ask what the reader no longer knows. If nothing, it stays deleted. A line earns its place by telling the reader something they did not know and can act on.

### (c) The label test

Every column header and every first-column row label: take the label and one value under it, and say what the value asserts. `Stopped? / no` fails: the reader cannot tell whether the line never stopped or whether stops are not recorded. Check every slide, not only the first where you find a problem: a label fixed on one slide survives on the next.

### (d) The decode test

List every term the deck uses without defining it — a method, a standard, an article of a rule, a material, an instrument, a unit, an acronym — and every line you can read only because you already know the field. Test each against the Audience block's vocabulary. Where the block does not settle it, the term carries its meaning in the same sentence or the next, the first time it appears.

**Your own familiarity is not evidence.** A line like `OTIF at 91%, against 96% target` will not look like a problem to you, and to a board that does not run logistics it is one. When in doubt, define it.

### (e) The page budget

Compare `pages` with the slides: one page for the cover plus one per line holding only `---` in `presentation.md`. More pages means a slide ran over. The PDF's text separates its pages with a form feed (`\f`): a page whose first line is not a slide title continues the page before it.

Then look for lines the renderer broke (`print-constraints.md` rule 10): compare the PDF's lines with the source lines; a printed line that is the beginning of a source line, with the rest on the next printed line, is a wrap. Fix it by breaking the source line by hand at a phrase boundary, or by removing words; never accept the renderer's break. Table headers and row labels need an eye of their own.

**Fix an overflow by removing a fact, a row or a whole slide, never by shortening a sentence.** Shortening takes out information first and rhythm last. Definitions go last, and a term that cannot keep its definition goes with it.

### (f) Provenance

For every number, comparison and conclusion, name where it came from: the vendor; a rule, a register or a public record; a primary source we read; **our arithmetic**; **our judgement**; someone's word.
- The first three are stated plainly.
- Our arithmetic and our judgement are **marked as ours inside the sentence**. An estimate or an inference written in the same voice as a published figure is a finding.
- Someone's word does not ship: a claim about a third party that cannot be sourced comes out.
- A cost, a size or a time inside the audience's own trade is a finding unless it was read somewhere.

**Then open every link and check that the page carries what the slide attributes to it.** It is the check that finds most, and it cannot be done from the markdown: a figure that is not on the linked page, a range quoted as a single number, a source that says the opposite of the sentence it supports. A technical reader clicks.

### (g) The actor

Every open question, every instruction and every "we": name who acts. A list of questions with no subject is homework. If we will get the answer, the slide says so; if it belongs to the reader's own conversation with a supplier, the slide says that.

Also a finding: anything that explains the audience their own trade, and any sentence about our own process, effort, corrections or diligence.

### (h) The cover

The cover prints the frontmatter `title`, and its `# ` line only when the frontmatter has none. The printed title names the subject (`copy-rules.md` rule 10) and fits on one line (`print-constraints.md` rule 10), and the `# ` line carries the same words, so the title is the same wherever the deck is read.

## The revision file

Write `presentation-revision.md`:

```markdown
# Revision — <date>

Run by: <a sub-agent | the drafting assistant, no sub-agent available>
Audience vocabulary: <from the brief | assumed, stated here>

## Findings

| Slide | Check | Line or label | What is wrong | Fix applied |
|---|---|---|---|---|
| 4 | d | "OTIF at 91%" | an acronym the board does not use | defined in the same line |
| 7 | c | `Stopped?` | the value cannot be read from the label | renamed "Line stopped during the test?" |

## Slides with no finding
<their numbers: a clean slide is a result>

## Not fixed, and why
<what is left and the reason; the section stays even when empty>
```

Then apply the fixes to `presentation.md`.

## Closing the step

1. Render again, as step 3 of `SKILL.md` does, and compare `pages` with the slide count once more: the fixes changed the page budget.
2. Tell the person, in their language: how many slides were checked and how many had findings; **the findings in full**, never "it looks fine"; and whether a sub-agent ran the checks.
3. A finding left open is the person's to accept or not: do not call the deck finished while one is open.

## Language

Chat in the person's language; the revision file in the deck's language.

**A deck in a second language is written again from the facts and gets its own revision.** The revision of the first language cannot see what the translation introduced: a label faithful word for word can be unreadable in the new language.

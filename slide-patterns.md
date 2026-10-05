# Slide patterns

The slide types that map cleanly to md2. Each one says when to use it, when not to, and gives a block to adapt. Pick the pattern that fits the beat; a slide holding one quote is right when the beat is one quote.

Every example below is invented: Northwind Traders is a wholesaler, and its people are invented too.

## 1. Cover

**When**: the first slide of every deck.

**Not**: bullets, charts or sources on the cover.

The cover prints the frontmatter `title`; the `# ` line repeats it, and is what the cover shows when the frontmatter has none.

```markdown
+++
title = "Q3 results for the Northwind board"
palette = "cool"
lang = "en"
+++

# Q3 results for the Northwind board

Revenue and costs against the Q3 plan, and the sales hire the board is asked to approve.

**Audience:** Board · **Date:** 2026-10-15
```

## 2. Section divider

**When**: a transition between two major parts, such as from the numbers to the ask. Two or three per deck at most.

**Not**: every few slides, and never a title alone: the line under it is required (`print-constraints.md` rule 6).

```markdown
## Revenue after three quarters is 6% below plan

Where the gap comes from, and what closing it in Q4 would take.
```

The title still states a takeaway. The line under it says what comes next, plainly, not as a teaser (`copy-rules.md` rule 7b).

## 2b. Chapter cover

**When**: the opening of each act of a deck in several parts, such as "Part 1 — The numbers", "Part 2 — The ask". One per act.

**Not**: as a transition inside an act, which is pattern 2. A chapter cover every few slides breaks the pace.

```markdown
:::chapter
# Where the money goes
The three cost lines that grew in Q3, and why.
:::
```

The subtitle is where a slogan creeps in, because a short line under a large title invites one. Write what the act contains: *"The three cost lines that grew in Q3, and why"*, not *"Three lines. One quarter. No excuses."* (`copy-rules.md` rule 7b).

## 3. Hero number

**When**: one number is the finding. Once or twice per deck.

**Not**: several large numbers on one slide; that is a table or a list.

```markdown
## Orders grew 21% on the year, the fastest since 2019

# +21%

Orders, Q3 2026 against Q3 2025. Source: Northwind order system, 2026-10-03 extract.
```

## 4. Bullet list

**When**: three to five parallel points the audience must keep. Most slides.

**Not**: more than six bullets; levels of abstraction mixed in one list.

```markdown
## Three changes would recover the Q4 margin

- **Freight**: move the Lyon route to the weekly consolidated truck, €4,200 a month
- **Returns**: inspect within two days instead of nine
- **Pricing**: end the 2024 discount on the 40 slowest items
```

## 5. Two columns

**When**: before and after, today and tomorrow, two options side by side. The contrast is the message.

**Not**: two halves that do not compare one to one; that is a table.

```markdown
## Reading orders from the mail cuts manual entry from 6 hours to 1 a week

:::columns

:::col
**Today**
- 6 hours a week keying orders
- 3% of lines keyed wrong

:::col
**From March**
- 1 hour a week checking orders
- Errors caught before the order is confirmed

:::
```

## 6. Quote

**When**: a customer or an expert confirms the point in their own words. Strongest just before the ask.

**Not**: a paraphrase, or a quote with no one behind it.

```markdown
## Customers who moved to weekly delivery are keeping it

> "We cut our own stock by a third and have not run out of anything since June."

— Elena Ferri, purchasing manager, Ferri Ristorazione (invented example)
```

## 7. Process

**When**: steps the audience will follow, in order.

**Not**: numbering steps that are not in sequence; use bullets.

```markdown
## The new supplier is onboarded in four steps over two weeks

1. **Day 1** — contract and price list loaded
2. **Days 2-5** — test orders on ten items
3. **Day 8** — first live order, checked line by line
4. **Day 14** — the supplier joins the weekly order run
```

## 8. Timeline

**When**: milestones over time, discrete dates and events.

**Not**: events that overlap; use a table with start and end.

```markdown
## The warehouse move completes in five months

| Month | Milestone |
|---|---|
| November | Lease signed, layout approved |
| December | Racking installed |
| January | Slow-moving stock moved |
| February | Picking runs from both sites |
| March | Old site closed |
```

## 9. One chart

**When**: a chart carries the whole message of the slide: a trend, a split, a ranking.

**Not**: a chart with a long paragraph or a table beside it (`print-constraints.md`). One chart, one short caption.

```markdown
## Freight cost per order rose for three quarters, then fell

Euros per order, from the carrier invoices.

:::chart line --title "Freight per order, €"
| Quarter | Freight |
|---|---|
| Q4 2025 | 11.2 |
| Q1 2026 | 12.0 |
| Q2 2026 | 12.9 |
| Q3 2026 | 11.6 |
:::
```

## 10. Table

**When**: a comparison along several dimensions: options, costs, indicators by segment. A table carries more text than a chart.

**Not**: a single number (pattern 3) or two things side by side (pattern 5).

```markdown
## Only the regional carrier delivers next day at under €12 an order

| Carrier | Next-day delivery | Cost per order | Pallet pickup |
|---|---|---|---|
| [Rapido Freight](https://example.com/rapido) | yes | €11.40 | yes |
| National Express Parcels | no | €9.80 | no |
| Contract van | yes | €14.10 | yes |

> The regional carrier is the only one meeting both conditions the sales team set.
```

## 11. Image

**When**: a diagram, a screenshot, a photo.

**Not**: an image with no caption. The image must be reachable by https: the renderer does not see the workspace.

```markdown
## Orders now arrive in one queue, whatever the channel

![Order flow from mail, portal and phone into one queue](https://example.com/order-flow.png)

*Mail, portal and phone orders all land in the same queue, checked by one person.*
```

## 12. People

**When**: the people behind the project. Six at most on one slide.

**Not**: twelve photos on one slide.

```markdown
## Two people run the project, with one day a week from finance

:::columns

:::col
**Marco Bellini** — operations lead
Twelve years running the Turin warehouse.

:::col
**Giulia Conti** — project manager
Led the 2024 move of the order system.

:::
```

## 13. Closing and ask

**When**: the last slide, always. The audience leaves with a decision, a contact or a next step.

**Not**: "Thank you" alone.

```markdown
## We ask the board to approve the hire by 31 October

- **Decision**: approve one sales hire, €58,000 a year, starting January
- **Next step**: the role is advertised the week after approval
- **Contact**: Giulia Conti, giulia.conti@example.com
```

## Which pattern for which beat

| Beat | Pattern |
|---|---|
| Open the deck | 1. Cover |
| Move to another part | 2. Section divider |
| Open an act | 2b. Chapter cover |
| Fix one number in memory | 3. Hero number |
| Three things to remember | 4. Bullet list |
| Today against tomorrow | 5. Two columns |
| A customer's own words | 6. Quote |
| Steps in order | 7. Process |
| Dates and milestones | 8. Timeline |
| Data to a conclusion | 9. One chart |
| A comparison on four or more dimensions | 10. Table |
| A diagram or screenshot | 11. Image |
| The team | 12. People |
| The ask | 13. Closing |

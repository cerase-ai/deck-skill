# Step 1 — the brief

Interview the person, then write one file, `presentation-brief.md`, in the workspace. The draft reads it to decide every slide: who the deck is for, what it must achieve, what it must contain, and how it looks.

If `presentation-brief.md` already exists, ask whether to replace it, add to it, or write a new file under another name. Never overwrite it silently.

## The interview

Ask in this order, one or two questions at a time, in the person's language. Skip nothing: when the person does not know, write `unknown` and move on, and the draft will ask again where it matters.

### 1. Audience

- Who is in the room: roles, seniority, function — a board, a management team, prospects, colleagues, a conference.
- What they already know about the subject.
- **Which terms this audience uses every day, and which they would have to look up.** Ask for the two lists by name. "They are experts" does not answer it: a person can run a logistics company for twenty years and never have met `OTIF` or `CMR` if those belong to the market the deck describes rather than to their job. Write the answer down as given. The draft tests every term against these lists, and without them it falls back on its own vocabulary, which is the one that produces an unreadable deck.
- What they care about: costs, risk, growth, a decision, a deadline.

### 2. Objective

The one outcome the deck exists for:
- **Decide**: the audience is asked to make a decision.
- **Approve or fund**: the audience is asked to approve or release a budget.
- **Update**: the audience needs a status, no decision.
- **Persuade**: change what the audience believes, or commit them to act later.
- **Inform or teach**: share knowledge with nothing asked.

For decide and approve: the exact ask, with amount and date when there are any.

### 3. How it will be used

- **Presented** by a speaker, **read alone** as a leave-behind, or **both**. This sets the density: presented means few words per slide and titles that carry the message; read alone means each slide must stand without a speaker; both means read-alone density with strong titles.
- **Orientation**: `landscape`, the default and right for almost every deck; `portrait` only for a text-heavy document meant to be printed and read like a report, or when the person asks.
- **Paper**: `A4`, the default, or `letter`.

### 4. Length

- A slide count, or a time slot: 10 minutes is 8 to 12 slides, 30 minutes 15 to 25, a board update 5 to 10.
- Any hard limit: "five minutes at most", "one page when printed".

### 5. Brand

- A palette among `default`, `warm`, `cool`, `mono`, `vivid`, `pastel`, or brand colours as hex codes, or a heading and body font, or CSS the person already has. None of these: `Brand: default`.
- A logo, only as an https URL of an image: a file in the workspace does not reach the renderer.
- The deck's language, when it differs from the person's.

### 6. Content that must be there

Most decks fail here, so ask for the facts before the draft has to guess them:
- **Numbers and data** the deck must carry, such as "orders grew from 1,200 to 1,450 in Q3".
- **Claims** the person wants to make, such as "we deliver in 24 hours where others take 48".
- **Sources** behind them: links, reports, internal documents.
- **Quotes**, with who said them.
- **Visuals** the person already has: charts, screenshots, diagrams.
- **Sections** the deck must contain.
- **What to avoid**: sensitive subjects, competitors not to name.

### 7. Tone

- Formal, neutral, informal or direct.
- One sentence the person thinks "sounds right" for this deck.

### 8. Confirm, then write

Summarise the brief in five to ten lines, ask the person to confirm, then write the file.

## Check before writing

- The audience is concrete: "the Northwind board: the CEO, the CFO and two investors" rather than "stakeholders".
- The objective is one of the five, not "it depends".
- The usage is one of the three.
- At least one item under content is filled: numbers, claims or sources.

If any of these is empty after the interview, ask one more targeted question before writing.

## The file

Write `presentation-brief.md` with these headings exactly, in short bullets:

```markdown
# Presentation brief

## Audience
- Who: ...
- What they know: ...
- Terms they use every day: ...
- Terms they would have to look up: ...
- What they care about: ...

## Objective
- Outcome: <decide|approve|update|persuade|inform>
- The ask: ...

## Format
- Usage: <presented|read alone|both>
- Density: ...
- Orientation: <landscape|portrait>
- Paper: <A4|letter>

## Length
- Slides: ...
- Time slot: ...
- Hard limits: ...

## Brand
- Brand: <default | palette name | colours, fonts or CSS as given>
- Logo: <https URL or none>
- Deck language: ...

## Content

### Numbers and data
- ...

### Claims
- ...

### Sources
- ...

### Quotes
- ...

### Visuals provided
- ...

### Required sections
- ...

### Avoid
- ...

## Tone
- Register: ...
- Reference sentence: "..."

## Notes
- ...
```

Then tell the person the brief is written and that the next step is the draft.

# Development Log — *Autonomous Weapons and the Laws of War*

**Companion website artefact · INT0053 Current Global Issues**
Yazan Anzharini · Ifran Zargar · Abdullah Farid

---

## Why this log exists

The finished website shows *what* we built. It does not show *how* we got
there, what we tried and abandoned, or why any given decision was made.
This log and the accompanying git history exist to make the development
process itself inspectable.

The repository holds five commits, one per development stage. Each commit
contains a complete, working version of the site as it stood at that point.
Any stage can be checked out and opened in a browser.

```bash
git log --oneline              # see all five stages
git checkout v0.3              # open the site as it was at stage 3
git diff v0.3 v0.4 -- index.html   # see exactly what changed and when
```

---

## Stage 1 — Skeleton and structure
`tag v0.1` · *build the site skeleton and content structure*

The first working prototype. The goal was to prove the structure before
committing to any content.

**Built:** a nine-section long-form structure mapped directly to the
research question — the question, definition, the law, the cases, the
debate, the accountability gap, the survey, the verdict, the references.
Dark editorial theme driven by CSS custom properties so the palette could
be retuned in one place. An interactive autonomy spectrum with four
clickable steps, so a visitor can explore the human-in-the-loop /
on-the-loop / out-of-the-loop distinction rather than just read about it.

**Technical decision — no framework, no build step.** The artefact has to
open from a USB stick at the exhibition with no internet connection. A
single hand-coded HTML file with inline CSS and JS guarantees that. React
or a static-site generator would have added a build pipeline and a
node_modules folder for no benefit a visitor would ever see.

**Deliberately left incomplete.** The survey percentages at this stage
(64/22/12/2) were invented sample values used only to prove the layout
worked; the real survey had not been run. The verdict section held a
written brief to ourselves rather than content, because we did not want to
write a conclusion before the evidence was in. The footer was marked
`BETA v0.1` so nobody could mistake it for finished work.

---

## Stage 2 — Retitling after group review
`tag v0.2` · *retitle the project after group review*

A two-line code change recorded as its own commit, because the reasoning
matters more than the diff.

The working title was *Machine Judgement*. In review we decided it read as
a documentary tagline rather than academic work — for a research artefact
assessed on analysis and evaluation, a title that sounds like marketing
sets the wrong expectation before a marker reads a single line.

**Options considered and rejected:**

| Title | Why rejected |
|---|---|
| *Out of the Loop* | Jargon — meaningless to a general visitor |
| *The Last Human Decision* | Editorialising; presumes our conclusion |
| *No One on Trial* | Presumes our conclusion even more strongly |
| *Machine Judgement* | The incumbent; stylish but vague |

**Decision:** *Autonomous Weapons and the Laws of War*. Descriptive,
neutral, and it names both halves of the research question. A visitor knows
what they are looking at within one second, and a marker can see the topic
and the legal framing from the title alone.

---

## Stage 3 — Primary research integrated
`tag v0.3` · *integrate primary research; add animated data reveal*

The survey of 50 foundation-programme students was complete. Placeholder
numbers came out, real findings went in, and the section was rebuilt around
them.

**Data integrated:** 91% insist someone must remain accountable · 87%
uncomfortable with a machine deciding to take a human life · 82% want human
approval on every decision to fire · 73% say only a human can carry the
moral weight · 84% found the Kargu-2 incident deeply concerning · 56% think
a machine would be worse than a soldier at distinction.

**Sophisticated use of digital technology.** Each bar is set to `width: 0`
in CSS and only animates to its true value once it scrolls into view, using
an `IntersectionObserver`. The percentage counts up on a
`requestAnimationFrame` loop with easeOutExpo timing, so the number and the
bar arrive together. Each bar fires once and then unobserves itself. There
is a static fallback for browsers without `IntersectionObserver`, and the
whole effect is disabled under `prefers-reduced-motion`.

This is not decoration. A reader meets each statistic one at a time, at
reading pace, instead of skimming a wall of numbers.

**Analysis rather than reporting.** We added a "What this tells us" block
arguing two things: that the 91% demand for accountability sitting against
the law's inability to deliver it *is* the accountability gap measured
empirically; and that the near-even 56% split on distinction shows our
respondents hold the moral objection more firmly than the technical one —
which bears directly on Sharkey (2012) versus Schmitt & Thurnher (2013).

We also wrote an explicit limitations note: n = 50, a single cohort,
self-selecting, and briefed before answering, therefore not generalisable.
It is better to state the weakness than to have a marker find it.

**Known incomplete at this stage.** Only two of the five accountability
mean ranks had been recovered from the results file (government/military
2.05, programmer 4.12); the middle three were marked `TBD` in the markup.
We began trialling a **Flourish** embed for the interactive ranking chart —
placeholder and loading state built, chart not yet made.

---

## Stage 4 — Third-party embed rejected; component built in-house
`tag v0.4` · *drop Flourish; build the accountability ladder in-house*

**This is the stage we would point a marker at.** It is a reversal — we
evaluated the stage 3 approach against how the artefact will actually be
used, and abandoned it.

### Why Flourish was rejected

1. **It needs a live internet connection.** The exhibition is assessed in
   person and we cannot guarantee wifi. A loading spinner where the
   analysis should be would be a visible failure in front of markers.
2. **It is a third-party account dependency.** If the embed is
   unpublished, rate-limited, or the free tier changes, the artefact breaks
   and we cannot fix it.
3. **Styling is constrained.** Matching our dark palette would have meant
   fighting their theme editor.
4. **It is weaker as evidence.** An embed demonstrates that we can use a
   tool. A component we wrote demonstrates that we can build one.

### What replaced it

A bespoke **blame ladder** in plain HTML, CSS and JavaScript:

- Five rungs in rank order, with the full dataset now recovered —
  government/military 2.05, operator 2.46, commander 2.49, manufacturer
  3.88, programmer 4.12.
- Bar length is **derived, not eyeballed**: `blame = (6 − mean) ÷ 5 × 100`.
  The formula sits in a code comment so it can be checked.
- Bars animate on scroll, staggered 110 ms apart so the ranking reveals
  itself from top to bottom.
- Each rung expands on click to explain why that party sits where it does —
  accordion behaviour, one panel open at a time.
- Built from real `<button>` elements with `aria-expanded` and
  `aria-controls`, so the component works by keyboard and screen reader.

### Analysis embedded in the design

- **Tier colouring follows the actual clustering in the data**, not a
  decorative gradient: the state alone at the top, operator and commander
  effectively tied, then manufacturer and programmer after the drop.
- Operator (2.46) and commander (2.49) are **0.03 apart**. At n = 50 that
  is noise, so we wrote them up as tied rather than pretending rank 2 beat
  rank 3.
- A callout marks the **1.39 jump** between rung 3 and rung 4 — the largest
  gap in the dataset. Respondents are not spreading blame evenly down a
  chain; they draw a hard line between those who decided to *use* the
  weapon and those who *built* it.
- The rung 5 explanation argues our own respondents were probably **wrong**
  to rank the programmer last, since in an autonomous system the targeting
  logic decides who counts as a target. Critiquing our own data rather than
  simply reporting it.
- A separate note explains that the machine itself was excluded from the
  ranking because it cannot be offered one in a courtroom either.

---

## Stage 5 — QA pass
`tag v1.0` · *fix layout bug, write verdict, trim scope*

### Bug found and fixed: ladder columns misaligned

**Symptom.** Rung numbers floated in empty space, the name and score were
pushed to the right, the `+` caret wrapped onto its own line, rows were
abnormally tall, and the blame bars were invisible.

**Cause.** The rule `.rung-head > * { position: relative; z-index: 1; }`,
added to lift the row text above the bar, also matched `.rung-fill`. That
overrode the bar's `position: absolute`, so it rejoined the grid flow,
claimed column one, and displaced every other column by one place.

**Fix.** Scoped the rule to `.rung-head > *:not(.rung-fill)`. We also gave
the bar `pointer-events: none` so it can never intercept a click, and a
brighter right edge so its length is readable at a glance. A regression
test now asserts that the bar is always the first child and always outside
the grid flow.

### Verdict written

The placeholder in section 08 was replaced with the group's evaluative
conclusion, arguing that the research question resolves in two layers:
compliance is not currently possible or verifiable as a matter of
engineering, but even a flawless machine would leave the accountability gap
untouched, because that is a problem of law and responsibility rather than
technology.

### Scope trimmed: visitor poll removed

The "add your own view" poll was cut. It collected nothing — with no
backend, votes vanished on refresh — and its percentages were still the
invented stage 1 placeholders, sitting directly beneath our real survey
data where the two could be confused. Removing it cost us an interactive
feature, but leaving fabricated numbers next to genuine research would have
been indefensible. Markup, CSS and JavaScript were all deleted rather than
commented out.

### Typography normalised

The verdict paragraphs were rendering at 19 px via `.large` while the rest
of the site runs at 17 px, which made the conclusion read as a different
document. Normalised to body size; `.large` is now used exactly once, as
the deliberate lede in section 01.

---

## Testing approach

Each stage was verified programmatically rather than by eye. The page's
real JavaScript was executed in a headless DOM with a stubbed
`IntersectionObserver`, which was then fired manually to confirm that:

- every animated bar starts at 0% and reaches exactly its target value;
- each bar animates once and then stops observing;
- the accordion opens, closes, and keeps only one panel open;
- `aria-expanded` and `aria-controls` stay correctly wired;
- bar lengths match the stated formula to within 0.15;
- and no regression breaks the autonomy spectrum or the reference list.

Every one of the five stages was re-tested after reconstruction to confirm
it parses, runs without JavaScript errors, and behaves as its commit
message claims.

---

## Known issues outstanding

1. **Sample size is inconsistent.** The verdict cites *45 respondents*
   while the findings section and the ladder cite *n = 50*. This must be
   reconciled before submission.
2. **Two verdict statistics are unsupported on the page** — 89% wanting
   regulation or a ban, and 20% confidence in governments — and would be
   stronger presented as bars in section 07.
3. Group member names and final imagery still to be added.

---

## Mapping to the assessment criteria

| Criterion | Where the evidence is |
|---|---|
| Evidence of development | Five commits, each a working version; `git diff` between any two tags shows exactly what changed |
| Very effective analysis and evaluation | Stage 4's rejection of Flourish with stated reasons; the 1.39 gap analysis; the rung 5 self-critique; the stage 3 limitations note |
| Highly sophisticated use of digital technology | `IntersectionObserver` scroll-triggered reveals, `requestAnimationFrame` count-up easing, a bespoke accessible accordion, derived data visualisation, reduced-motion and legacy-browser fallbacks |
| Professional standard | Semantic HTML, ARIA wiring, keyboard operability, no external dependencies, programmatic regression testing, documented known issues |

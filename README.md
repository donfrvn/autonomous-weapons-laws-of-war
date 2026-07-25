# Autonomous Weapons and the Laws of War

Companion website artefact for **INT0053 Current Global Issues**.

> To what extent can lethal autonomous weapons systems (LAWS) comply with
> the principles of international humanitarian law, or do they fundamentally
> undermine the laws of armed conflict?

**Group:** Yazan Anzharini · Ifran Zargar · Abdullah Farid

---

## Running it

Open `index.html` in any modern browser. 

---

## What's in here

| File | Purpose |
|---|---|
| `index.html` | The complete artefact — HTML, CSS and JavaScript in one file |
| `DEVELOPMENT_LOG.md` | Stage-by-stage account of how it was built and why |
| `.gitignore` | Standard exclusions |

---

## The five development stages

| Tag | Stage | Summary |
|---|---|---|
| `v0.1` | 1 | Skeleton and structure; placeholder data |
| `v0.2` | 2 | Retitled after group review |
| `v0.3` | 3 | Primary research integrated; scroll-animated data reveals |
| `v0.4` | 4 | Flourish embed rejected; accountability ladder built in-house |
| `v1.0` | 5 | QA pass — layout bug fixed, verdict written, scope trimmed |

Inspect any stage:

```bash
git log --oneline                    # all five stages
git show v0.4                        # full reasoning for stage 4
git checkout v0.3 && open index.html # browse the site as it was
git diff v0.3 v0.4 -- index.html     # what changed between stages
git checkout main                    # back to the finished version
```

Each tag is a complete, working version of the site.

> The history was reconstructed retrospectively and committed in one
> session. The stages and their order are accurate; the timestamps were
> assigned at creation. See the note at the top of `DEVELOPMENT_LOG.md`.

---

## Site structure

1. The question — research question and framing
2. Definition — what an autonomous weapon is, with an interactive autonomy spectrum
3. The law — the four IHL principles
4. In the field — Libya 2020, Ukraine, Gaza, the CCW talks
5. The debate — the case for and against, side by side
6. The gap — the interactive accountability blame ladder
7. Findings — our primary research, with animated result bars and analysis
8. Our verdict — the group's evaluative conclusion
9. Sources — APA 7th edition reference list

---

## Technical notes

**Animated data reveals.** Result bars and ladder rungs start at zero width
and animate to their true values only when scrolled into view, via
`IntersectionObserver`. Percentages count up on a `requestAnimationFrame`
loop with easeOutExpo easing so the number and the bar land together. Each
element animates once. Static fallback for browsers without
`IntersectionObserver`; the whole effect is disabled under
`prefers-reduced-motion`.

**The blame ladder.** Bar lengths are derived from the survey's mean ranks
using `blame = (6 − mean) ÷ 5 × 100`, not set by hand. Rungs are real
`<button>` elements with `aria-expanded` and `aria-controls`, so the
component is fully keyboard and screen-reader operable.

**No external assets.** No CDN, no web fonts, no analytics, no third-party
embeds. Everything the page needs is in the file.

---

## Known issues

See the *Known issues outstanding* section of `DEVELOPMENT_LOG.md`.

The sample-size inconsistency has been resolved: **n = 45** (completed
responses) throughout. Remaining items are group member names and final
imagery.

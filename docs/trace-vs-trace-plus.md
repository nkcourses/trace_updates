# Trace vs Trace+ — the boundary, on one page

Rewritten 12 Aug 2026 against **Trace 0.19.0, running in Premiere**. The previous version compared a
spec to a spec; this one compares a shipping panel to a plan. **Trace+ is still a spec with no code**,
so its column is a promise.

Supersedes the tables in `feature-roadmap.md`. Sources: `../../docs/trace-and-wrap.md` (written when
Trace+ was called Wrap), `../../docs/extension-ideas.md` §3.

---

## The one sentence

> **Trace answers about ONE clip. Trace+ answers about MANY.**

🔴 **Settled 12 Aug 2026, and this is the version that ships.** Trace was cut back to a single
selected clip; sequence view and the source coverage bar both moved to `../../Trace+/`, as working
code, in a plugin that loads. The free panel gives you the whole answer about one thing. The paid one
gives you the same answer about a sequence's worth, or a project's worth, plus the analysis on top.

Same engine underneath, different question on top. That is the whole design.

### 🔴 The old wording was wrong, and wrong in the direction that costs money

It used to read *"Trace answers about a clip you selected. Trace+ answers about the project."*
**Trace already reads the entire project** — `buildIndex()` walks every sequence, every track, every
track item, which is exactly how it can say "used in four sequences". Describing Trace+ as "the one
that looks at the project" is therefore false, and it hands a reviewer the reply *"but Trace already
does that."*

The axis is **one clip versus every clip**, never **one sequence versus the project**:

| | Trace | Trace+ |
|---|---|---|
| **Searches** | The whole project | The whole project |
| **You give it** | One clip | Nothing |
| **It answers for** | That clip, everywhere | Every clip in the project |
| **The question** | Where is *this* used? | What is used *nowhere*? |
| **Then** | Selects them for you | Files the unused away, and prices the difference |

⚠️ **The same error is live in the headline.** *"See every used clip across your entire project"*
describes Trace+, not Trace — it promises a list of all the project's clips. Corrected everywhere to
**"See every USE OF A clip across your entire project"**. One word, and it is the whole boundary.

---

## 🔴 "Is Trace getting too good to sell Trace+?" — the honest answer

Asked 12 Aug 2026, and it deserves a straight answer rather than reassurance.

**The fear is half-right, and it is pointed at the wrong thing.**

### What is NOT the risk

**Trace being excellent is not the risk.** Nobody has ever paid for a better list. People pay for
three things and Trace has none of them:

1. **A number with money attached.** *"Your project folder is 840GB. 610GB of it is unused."*
2. **A deliverable someone else is demanding.** A cue sheet. Music licensing screen time.
3. **A chore with a deadline.** Archive this job before Friday.

Every single thing Trace does is **lookup**, and lookup is a mid-edit convenience nobody invoices
for. Trace+ is **end-of-job work**, and end-of-job work has an invoice-shaped trigger behind it. A
person who has used Trace forty times has been taught the shape of the problem and has never once
been given the thing that solves it at scale.

And the inverse matters more than it looks: **a weak free tool does not protect a paid one — it just
does not get downloaded.** The lead magnet's only job is the email address. A mediocre Trace collects
fewer addresses and therefore sells fewer Trace+ licences than an excellent one. Quality is not the
lever here.

### What IS the risk, in order

1. **🔴 Positioning Trace+ as "more Trace."** This is the real danger and it is already flagged below
   in *Renaming Wrap → Trace+*. Sold as *"Trace, but for the whole project"*, a great free Trace does
   kill it — it reads as a bigger helping of something you already have enough of. Sold as *"your
   archive is four times the size it needs to be and nobody can tell what mattered"*, Trace being
   excellent is pure credibility for the same author. **The name is the tier. The pitch is still
   end-of-job cleanup.**
2. **Where Trace's own UI now points.** Sequence view answers "what does this sequence reuse?" The
   obvious next request from a user is "can it do that for the whole project?" — which is Trace+'s
   headline, now one dropdown away. The feature is fine; the slope under it is the problem.
3. **Copy list growing into a file.** Plain text on a clipboard is a convenience. A saved CSV is the
   thin end of the report, and the report is Trace+.

### The one feature that already crossed the line

Applying the boundary test to everything Trace has gained since this doc was first written:

| Added since the spec | Needs a selection first? | Verdict |
|---|---|---|
| Source coverage bar | Yes — a clip | ✅ Trace |
| Markers at every use | Yes | ✅ Trace |
| Copy list (plain text) | Yes | ✅ Trace, **while it stays clipboard-only** |
| Source Monitor with in/out | Yes | ✅ Trace |
| Multi-select picks | Yes | ✅ Trace |
| **Sequence view — what this sequence reuses** | **No** | ✅ **moved to Trace+ 12 Aug** |

**Sequence view was the one foot across, and it has been moved.** It answered about MANY clips from
NO selection, which is Trace+'s shape exactly. Trace is now one clip, always — which also means the
free panel can be described in a single sentence with nothing left over, and every future
"could it also show…" request has an obvious answer.

### The three lines to defend — and none of them is about quality

> 1. **Trace never aggregates across the project.** No "all clips", no project totals, no
>    project-wide anything.
> 2. **Trace never moves, files, colours or deletes.** Markers and in/out points are the ceiling.
> 3. **Trace never writes a file.** Clipboard text is the ceiling.

Break any one of those and Trace+ has nothing to sell. Keep all three and Trace can be as good as it
likes.

---

## Side by side

| | **Trace** (free) | **Trace+** (paid) |
|---|---|---|
| **The question** | "Where is *this* clip used?" | "What does this project no longer need?" |
| **What you give it** | One clip, selected in the Project panel | Nothing. It reads the whole project |
| **What you get back** | A list, and a live selection in the timeline | A number, a report, and an opt-in tidy-up |
| **Does it change the project?** | Only if you ask: markers, and in/out points | Yes — moves items into `_UNUSED`, colours them. Never deletes |
| **When you reach for it** | Mid-edit: before deleting, before replacing temp footage | End of job: before archiving |
| **How often** | A handful of times per project | Once, at the end |
| **What makes someone pay** | Nothing — it is the lead magnet | A deadline, a disk bill, or a cue sheet someone is waiting on |
| **Status** | **0.19.0, running in Premiere** | Spec only. No code |

---

## Feature by feature

🔴 **TRACE+ IS A SUPERSET. Every ✅ in the Trace column is a ✅ in the Trace+ column, always.**
There is no row where the free panel can do something the paid one cannot — a customer who upgrades
must never lose a habit. If a table here ever shows Trace ✅ against Trace+ —, that is a bug in the
table or a bug in the plan, and it gets fixed before anything ships.

✅ has it · 🚫 must never have it

### Looking up one clip

| | Trace | Trace+ |
|---|---|---|
| Every use, across every sequence | ✅ | ✅ |
| Timecode, duration and track per use | ✅ | ✅ |
| Resolves through nested sequences | ✅ | ✅ shared engine |
| Click a row → jump the playhead there | ✅ | ✅ |
| **Select every instance at once** | ✅ **Trace's headline** | ✅ |
| Next / previous stepping | ✅ | ✅ |
| Totals — "9 uses · 4 sequences · 3m 12s" | ✅ | ✅ |
| Open in the Source Monitor at that use | ✅ | ✅ |
| Subclips matched by media file | ✅ | ✅ |
| One colour per use, on the row | ✅ | ✅ |
| **Source coverage bar** — which *parts* of a clip you've used, % used, longest untouched stretch | 🚫 **moved out 12 Aug** | ✅ |

### Looking at one sequence

| | Trace | Trace+ |
|---|---|---|
| Which clips this sequence reuses | 🚫 **moved out 12 Aug** | ✅ |
| Hide archived sequences | ✅ | ✅ |

### Looking at the whole project — **this is the line**

| | Trace | Trace+ |
|---|---|---|
| **Unused media, project-wide** | 🚫 **never** | ✅ **Trace+'s headline** |
| Duplicate imports — same file, several project items | 🚫 | ✅ |
| Empty bins · orphan sequences | 🚫 | ✅ |
| Offline media, project-wide | 🚫 — flags the one in your hand | ✅ |
| **Screen time for *every* clip** (cue sheets, licensing) | 🚫 | ✅ |
| **What your archive would weigh without the unused media** | 🚫 | ✅ *the sentence that sells it* |

### Acting on what you found

| | Trace | Trace+ |
|---|---|---|
| Markers at every use | ✅ | ✅ |
| Set in/out to the used range | ✅ | ✅ |
| **Move unused media to `_UNUSED`, one undo step** | 🚫 **never** | ✅ |
| Colour-label what is unused | 🚫 | ✅ |
| Delete anything | 🚫 | 🚫 — **neither of them, ever** |

### Getting it out

| | Trace | Trace+ |
|---|---|---|
| Copy the visible list as plain text | ✅ | ✅ |
| **CSV / PDF report** | 🚫 **never** | ✅ |
| Cue sheet export | 🚫 | ✅ |

### Staying current

| | Trace | Trace+ |
|---|---|---|
| Re-scans automatically as you edit | ✅ | ✅ |
| Manual **Re-scan project** | ✅ | ✅ |

---

## 🔴 What was moved OUT of Trace, and what was considered and refused

### ✅ Moved out: the source coverage bar (12 Aug 2026, Trace 0.19.0)

It drew the whole source clip with the used parts filled in, one coloured segment per use, and read
*"68% of the source used · longest untouched 4m 12s"*. **It was the best thing in the panel**, which
is exactly the argument for moving it: it is the one feature people would have paid for, sitting in
the free tier.

It also fails the test cleanly. Trace is a **lookup** — *where is this?* The coverage bar is an
**analysis** — *how much of this have I used, and what have I never looked at?* That is a different
kind of answer about the same clip, and analysis is what Trace+ sells.

What was kept: **the colour**. One hue per use on the chip at the start of each row. It costs
nothing, it makes a nine-row list countable at a glance, and it was liked before the bar existed.

🔴 **`U.sourceCoverage()` STAYS in `js/usage.js`, fully tested, and Trace simply stops calling it.**
usage.js is the shared engine and Trace+ imports it verbatim, so deleting it would mean writing it
twice. `test/smoke.js` now fails if anything in Trace calls it again.

### 🚫 Refused: making Trace manual-refresh and Trace+ auto-refresh

Proposed 12 Aug as a way to widen the gap. **Recommended against, and the reasoning generalises.**

There is a difference between making the free tool **narrower** and making it **wrong**, and only
one of them is safe:

| | Narrower | Wrong |
|---|---|---|
| Example | The coverage bar moves to Trace+ | Trace stops noticing you deleted a clip |
| What the user sees | A feature they do not have | **A number that is a lie** |
| What they call it | "the paid one does more" | **"it's broken"** |
| Where it ends up | An upgrade page | A one-star review, and a support email you have to answer |

Trace's *entire* pitch is that its numbers are right. A panel that says "7 uses" after you deleted
three is not a limited panel, it is a broken one — and nobody upgrades to fix a bug, they uninstall.
The auto re-scan is **correctness**, not a feature, and it costs about 40 lines against an index that
rebuilds in 71ms.

Worse, it would be invisible as a gate. Nobody reads a stale panel and thinks *"ah, this must be the
free tier"* — the free/paid difference has to be legible at the moment it bites, and a wrong number
is not legible, it is just wrong.

**The four levers that ARE safe**, in the order they are worth using:

1. **Depth** — Trace looks it up, Trace+ analyses it. *(The coverage bar. This is the best lever and
   it is the one just used.)*
2. **Scale** — one clip versus all four hundred.
3. **Action** — Trace reads, Trace+ tidies up.
4. **Format** — Trace copies text, Trace+ writes a file.

Every one of those removes a **capability**. None of them removes **accuracy**. That is the rule:

> 🔴 **Narrow the free tool's scope. Never degrade its correctness.**

If a live project-wide watcher is wanted as a Trace+ differentiator, it is available honestly:
Trace watches for changes to answer about *one clip*, which is cheap. Trace+ keeping a live
project-wide unused-media count current as you edit is genuinely more work and genuinely more
valuable — and it is *additional*, not *withheld*.

## The shared engine

`js/usage.js` is written to be imported by both, verbatim. No DOM, and `ppro` is injected rather than
required, so it can be driven by stubs.

| | |
|---|---|
| `buildIndex()` | **Identical for both.** Walks every sequence × track × item, maps source id → instances |
| `resolveUsages(id)` | **Trace's question.** Look up one id, resolve nests, return the places |
| *Trace+ adds* | "Which project items have **zero** entries in `bySource`?" — that is unused media, and it is about four lines |

**That four lines is the whole risk, and it is why the boundary has to be a rule rather than a
preference.** Trace+ needs no new API call. Everything is proven. The gap between the two products is
a UI and a report — which is also why the temptation to bolt it on will keep coming back.

---

## 🔴 The test for any future feature

> **Does answering it require the user to have selected something first?**
>
> **No → it belongs in Trace+.** Not "it would be a nice extra in Trace". In Trace+.

Applied:

- "Show me every place this music cue is used" → needs a selection → **Trace** ✅
- "Which of my 400 clips are used nowhere?" → no selection → **Trace+**
- "Export this list to CSV" → the list needs a selection, but the *value* is the project-wide report,
  and Sequence Index already exports one sequence natively → **Trace+**
- "Total screen time" for the selected clip → **Trace** ✅ (built)
- "Total screen time for everything" → **Trace+**

And a second test, for anything that passes the first:

> **Does Trace still explain itself in one sentence with this in it?**
> **No → cut it.**

---

## 🔴 Renaming Wrap → Trace+ is not just a rename

Decided 12 Aug 2026. It is right for the funnel — "Trace shows you where one clip is, Trace+ shows
you all four hundred" is a legible upgrade path in a way two unrelated product names never were.

### ① The pain in the pitch has to survive the name

Wrap's pitch was **"your archive is four times the size it needs to be and nobody can tell what
mattered."** That is a money-shaped pain. "Trace+" implies *more Trace* — more of a free lookup tool
— which is a weaker thing to charge for. **This is the same point as the cannibalisation answer at
the top of this page, and it is the single most important line in the document.**

### ② Two separate plugins — ✅ DECIDED

| | One plugin, two tiers | **Two separate plugins** ✅ |
|---|---|---|
| Install | One install, unlock in place | Two installs |
| Licence | Needs validation in the free panel | Only Trace+ needs it |
| Free-user experience | Sees locked features — good funnel, worse first impression | Clean, but the upgrade is invisible |
| Code | One codebase | Shared `js/usage.js`, two shells |

Chosen: **two plugins.** No licence code lives in Trace. Note that Trace's manifest *does* now carry
a `network` permission — added for update checks only, GET only, two allowed hosts, enforced by
`test/smoke.js`. "Fully offline" is no longer one of Trace's claims.

### ③ The boundary rule is unchanged

A tier name makes the temptation *worse*, not better — "it's the same plugin, why not just show
unused media greyed out" is exactly how a paid feature leaks into a free one.

---

## What is left to build in Trace, and then stop

| Feature | Why it belongs here |
|---|---|
| **First-run tour** | Not a feature — a requirement. Four gestures on a row, and select-all is invisible without it. |
| **Multicam resolution** | Correctness, not scope — *if* the sequence route works. Merged clips are unbuildable by anyone; see `../../docs/premiere-uxp-api.md`. |
| **Three dock shapes + error states** | Correctness. |
| **Timeline → panel lookup** | Click a clip in the timeline, see every *other* place that source is used. Needs a selection, answers about one clip. Confirmed possible 12 Aug. Optional. |

**Trace is feature-complete after those.** Everything remaining on `launch.md` is launch, not code.

## ✂️ Cut from Trace entirely — not deferred, cut

| Cut | Why |
|---|---|
| **Marker colours** | A second transaction and a second undo step per sequence, for cosmetics. |
| **Multi-clip combined view** | "Showing 1 of 3 selected" is honest and enough. |
| **Retime / transition badges** | Noise on a row already carrying four things. |
| **Group / sort toggle** · **text-size control** | Settings, and Trace's pitch is that it has none. |
| **Persist the index** | Dead on arrival: 201 clips index in 71ms. |
| **CSV export of the current list** | Looks free. It is the thin end of the report, which is Trace+. |

---

# Working on both without drift — the process

Asked 12 Aug 2026: *"if we work on Trace+ and then come back to Trace with future releases, how do I
easily communicate what is different and what matches?"* Four mechanisms, in the order they pay off.
The first one is the only one that is not optional.

## ① One engine, shared physically — not by copy-paste

```
Premiere Extensions/
  shared/
    usage.js          🔴 THE MASTER. Edited here and nowhere else.
    timecode.js       🔴 THE MASTER.
    VERSION           engine version, e.g. "engine 1.4.0"
  Trace/js/usage.js       ← a copy, with the master's hash in a header comment
  Trace+/js/usage.js      ← the same copy, same hash
```

A tiny `shared/sync.py` copies the master into both plugins and stamps the hash. **Each plugin's
`test/smoke.js` fails if its copy's hash does not match the master.** That is the whole mechanism,
and it is worth building on day one of Trace+.

Why physically and not by discipline: the engine is the one thing that *must* be identical, it is the
thing most likely to be edited in a hurry in whichever plugin is open, and a divergence is silent —
two panels giving different answers about the same project, with no error anywhere. Everything else
on this list is a convention people can forget. This one cannot be forgotten because the build stops.

**Print the engine version in both scan reports.** When a bug arrives you need to know which engine
produced the number, not just which product.

## ② Build DOWN, never UP

> 🔴 **Every feature is built in Trace+ first, then back-ported to Trace if — and only if — the
> boundary test says it belongs there.**

This is the direct answer to the question, and it is the opposite of the instinct. Building in Trace
first and "adding it to Trace+ later" is exactly how Trace ended up with a coverage bar that had to
be taken out again — a thing users had, and lost. Building in Trace+ first means the free panel can
only ever *gain*, and the superset rule holds by construction rather than by review.

The one exception is a **correctness** fix, which lands in the shared engine and is therefore in both
the same day. That is the point of ①.

## ③ This file is the single source of truth for the matrix

Not a copy in each help page. The tables above are the spec; both products' help pages link here
rather than restating, so there is exactly one place to be wrong.

**Add the row to the table BEFORE writing the feature**, with its column decided. Two minutes, and it
is the moment the boundary test actually gets applied — after the code exists, every argument becomes
"but it is already built".

## ④ One release-note convention, two feeds

Both plugins already read an `updates.json` and both help pages render GitHub releases. Tag every
line so a reader can see the relationship at a glance:

```
- [both]   Nests inside nests now resolve to six levels.
- [trace]  Alt-click copies the clip name for the Project panel search.
- [trace+] Archive estimate now excludes proxies.
```

`[both]` entries are almost always shared-engine changes, which is a useful smell test: a `[both]`
line that did *not* come from `shared/` is worth a second look, because it means the same thing got
written twice.

## The two questions, on every feature, forever

> 1. **Does answering it require the user to have selected something first?**
>    No → **Trace+**.
> 2. **Is it a lookup, or an analysis?**
>    Analysis → **Trace+**.

And the rule that governs how the gap is widened:

> 🔴 **Narrow the free tool's scope. Never degrade its correctness.**

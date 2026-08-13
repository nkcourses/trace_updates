# Trace — what is left

Written 12 Aug 2026, against **0.5.0**. Reasoning behind the choices is in `feature-roadmap.md`;
this page is just the list.

**Where it actually stands:** the panel does more than v1 was scoped to do, and it has been run on a
real project. What has not started is the *release* — and `trace-go-no-go.md` puts the plugin at about
40% of the job. Everything in §4 below is the other 60%.

---

## ✅ Done (0.1.0 → 0.5.0)

Follows the Project panel selection · every usage across every sequence · totals · **select all
instances in a sequence** (verified working on a real project) · click-to-jump · next/previous ·
hide-archived · used-nowhere · nest resolution with honest `via` labelling · A/V pairing · coloured
per-use coverage bar with matching row chips · sequence mode with a repeats filter · where-is-this-
sequence-nested · mark all uses · open in Source Monitor · alt-click to locate · offline and
adjustment-layer flags · live auto-rebuild on every project change · scan report.

Measured: **19 sequences, 194 clips, 68ms.** The acceptance test was 5 seconds.

---

## 1 · Correctness — the only thing that should block more features

- [ ] 🔴 **Subclips.** Selecting a master whose *subclip* was cut in still reports **0 uses**. This is
      the most likely first bug report and it reads as a broken plugin, because the user knows they
      used it. Needs probe 6 answered first: what does `getProjectItem()` return for a subclip?
- [ ] 🔴 **Merged clips and multicam.** Same family, same silence. `isMergedClip()` and
      `isMulticamClip()` exist; the behaviour has to be *decided* and *stated in the UI*, not guessed.
- [ ] **Confirm the playhead lands on the exact frame** — is `getStartTime()` absolute sequence time
      including `getZeroPoint()`? If it lands short by exactly the sequence start timecode, that
      assumption is the bug.
- [ ] **Confirm `TickTime.createWithTicks` exists.** `tickTimeFrom()` falls back to
      `createWithSeconds`, which loses precision on long timelines. Add the answer to the scan report.
- [ ] **Three dock shapes.** Tall narrow strip, short wide bar under the timeline, small floating
      square. Never tested. Every overlap bug in VersionUp only appeared at small dock sizes.

## 2 · Features worth building

- [ ] 🟢 **Timeline → panel lookup.** Click a clip in the timeline, see every *other* place that
      source is used. **Confirmed possible on 12 Aug** — `sequence.getSelection()` is real at 25.6 and
      `SequenceEvent.SELECTION_CHANGED` exists. This was written off as impossible for eight days
      because "cannot *set* the Project panel selection" was misread as "cannot *read* the timeline
      selection". Probably the most useful thing left.
- [ ] 🔴 **First-run tour.** Standing order: *a shipped feature nobody can find is not shipped.*
      "Select all N in this sequence" and alt-click-to-locate are both invisible. The status-line hint
      is a stopgap. Port `Version Up/js/tour.js`.
- [ ] **Multi-clip.** Five clips selected → combined picture. Today it shows the first and says so.
- [ ] **Frame the use** — set sequence in/out around an instance. The workaround for not being able
      to zoom the timeline, which is the one navigation limit that will annoy people.
- [ ] **Colour the markers.** Needs a second transaction per sequence, because
      `createSetColorByIndexAction` lives on a Marker that does not exist until the first commits.
- [ ] Retime and transition badges per instance.

## 3 · 🔴 One decision, and it is overdue

**Trace has no `network` permission, so there is no update check and no in-panel feedback form.**
That was decided deliberately — fully offline is what makes a free tool safe to install — but the
consequence has not been handled:

- Every bug you ship is **permanent** for anyone who does not happen to re-download.
- There is no route for a user to tell you something is broken.
- You lose the second touchpoint with the mailing list that an update notice gives you.

VersionUp already solves this with `updates.json` on `raw.githubusercontent.com`. Adding that one
domain costs the "fully offline" line and buys a working update path.

- [ ] **Decide:** stay fully offline, or add the update check. Not deciding *is* deciding, badly.
- [ ] Either way: a support email has to be visible somewhere in the panel.

## 4 · Shipping — the 60% that is not code

- [ ] **The demo GIF.** Still the highest-leverage item on the whole project, still not done.
      `Version Up/brand/demo_kit.py` + `make-demos.py`. The mark now exists to put in it, and
      "select all 7 uses at once" is the shot.
- [ ] Wire `brand/icons/` into `manifest.json` — the PNGs exist, the block is deliberately off until
      the panel is known to load. It is known to load now.
- [ ] Package the `.ccx` (UDT → ••• → Package).
- [ ] Install guide PDF + screenshots (`Version Up/brand/make-install-pdf.py`).
- [ ] Landing page with the GIF, the one-liner and an email field.
      🔴 **This is the only step that measures the actual goal.** Trace exists to collect email
      addresses, so count email addresses — against a number set in advance.
- [ ] Store copy, licence page, terms, refunds-equivalent.
- [ ] Launch email + the Video Usage tutorial video (free list growth from research already paid for).

## 5 · Validation that never ran

Steps 1–5 of `trace-checklist.md` were skipped to build. They are cheaper now, not more expensive —
and step 3 is the one that tells you whether the one-liner survives contact.

- [ ] Post the GIF on the open Adobe idea thread **"Better access to clip usage"** — 12 people already
      raised their hand for exactly this.
- [ ] r/editors, r/premiere, one editors' Facebook group.
- [ ] Add "what's the next thing you'd want automated?" to the VersionUp buyer email.
- [ ] Put the page up and set the pass/fail number **before** you can move it.

---

## If you only do three things

1. **Subclips.** It is the one bug that makes the tool look broken to someone who knows better.
2. **The demo GIF.** An afternoon, and it is the entire marketing.
3. **Decide the network question.** It gets more expensive to change after launch, not less.

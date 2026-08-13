# Trace — what else it could do

Written 12 Aug 2026. Updated for **0.2.0**, which is still **not yet loaded into Premiere once**.

✅ rows shipped in 0.2.0. Everything still open is below them.

Every row below is checked against `../../docs/premiere-uxp-api.md`. Effort is calendar time at the
pace this repo actually moves, not optimistic hours.

🔴 **Read this first: the line against Trace+.** *Trace answers about a clip you selected. Trace+ answers
about the project.* Section 4 is the list of things that would feel like obvious wins and would
destroy the paid product. The shared engine is `js/usage.js`; the different question goes on top of
it, in Trace+, not inside Trace.

---

## 1 · Correctness — finish these before adding anything

These are not features. They are the difference between a tool that is trusted and one that gets
uninstalled, and every one of them fails **silently**.

| # | Feature | Why it earns its place | API | Effort | Verdict |
|---|---|---|---|---|---|
| 1.1 | **Subclip resolution** | A subclip cut into a timeline reports the subclip, not its master. Select the master today and Trace says **0 uses** — reads as broken, and the user knows they used it. Most likely first bug report. | `ClipProjectItem` → walk to the parent media; `getInPoint`/`getOutPoint` to confirm overlap | 1 day | 🔴 **Do first** |
| 1.2 | **Merged & multicam clips** | Same family. Component clips of a merged clip, and angles inside a multicam source sequence, both report nothing. | `isMergedClip()`, `isMulticamClip()` ✅ 25.6 | 1 day | 🔴 Do first |
| 1.3 | ✅ **DONE 0.2.0** — **Pair A and V of one clip** | A synced clip on V1+A1 is one use to a human and two rows to Trace. Inflates every count on the totals line — the number that is the whole pitch. | Group instances sharing `sourceId` + overlapping start/end across kinds | ½ day | 🔴 Do first |
| 1.4 | ✅ **DONE 0.2.0** — **Flag offline uses** | "9 uses" means something different when three are offline. Cheap, and it is the moment people care most. | `await clip.isOffline()` ✅ 25.6 | 1 hour | ✅ Cheap |
| 1.5 | ✅ **DONE 0.2.0** — **Adjustment layers and disabled clips** | Partly done — disabled shows. Adjustment layers should be excluded or marked, never counted as a normal use. | `isAdjustmentLayer()`, `isDisabled()` ✅ 25.6 | 1 hour | ✅ Cheap |

---

## 2 · The cut line from the original checklist

Already agreed as "build only if the first list is done".

| # | Feature | Why it earns its place | API | Effort | Verdict |
|---|---|---|---|---|---|
| 2.1 | ✅ **DONE 0.2.0** — **Mark all uses** | A coloured marker at every usage, one undo step. The only visual flag that **survives the panel losing focus** — a selection does not. This is the thing you were reaching for when you said "highlight in the timeline". | `Markers.getMarkers(seq)` → `createAddMarkerAction(name, type, start, duration, comments)` + `createSetColorByIndexAction()` ✅ 25.6, one transaction | 1 day | ✅ Best of the cut line |
| 2.2 | **Multi-clip** | Select five clips in the Project panel, see the combined picture. Today Trace shows the first and says so. | Already have the selection array; the work is the UI | 1 day | ⚠️ After §1 |
| 2.3 | ✅ **DONE 0.2.0** — **Open in Source Monitor** | Two clicks saved, and it completes the "go and look at it" loop. | `SourceMonitor.openProjectItem(item)` ✅ 25.6 | 1 hour | ✅ Cheap |
| 2.4 | Step through instances | **Done in 0.1.0** — next/previous walks the visible list. | — | — | ✅ Shipped |

⚠️ `SourceMonitor.setPosition()` is **26.3**, so at a 25.6 floor you can open the source clip but not
park it on the used frame. Do not promise that in copy.

---

## 3 · New ideas that stay on Trace's side of the line

| # | Feature | Why it earns its place | API | Effort | Verdict |
|---|---|---|---|---|---|
| 3.1 | ✅ **DONE 0.2.0** — **Source coverage bar** | A thin bar representing the whole source clip, with the used portions filled in. Answers *"which parts of this 40-minute interview have I actually used, and which have I never touched?"* — **nothing native or third-party does this**, it needs no new data, and it is the best still image in the whole product. | `getInPoint()` / `getOutPoint()` per instance, already indexed ✅ 25.6 | 1–2 days | 🟢 **The one I would build** |
| 3.2 | ✅ **DONE 0.2.0** — **Where is this SEQUENCE used?** | Select a sequence in the Project panel and see every master it is nested inside. The reverse index already contains the answer — sequences are project items too, so this is mostly a UI branch. Native's answer here is unverified and probably absent. | Already in `index.bySource` | ½ day | 🟢 High value, near-free |
| 3.3 | **Frame the use** | Set the sequence in/out around the selected instance, so the timeline shows just that use. **This is the workaround for not being able to zoom or scroll the timeline** — the one navigation limit that will otherwise annoy people. | `sequence.createSetInPointAction()` / `createSetOutPointAction()` ✅ 25.6 | ½ day | 🟢 Fixes a real papercut |
| 3.4 | **Timeline → panel reverse lookup** | Click a clip in the timeline, Trace shows every *other* place that source is used. Would be the single most useful thing in the tool. ⚠️ **Needs a probe:** `SequenceEvent.SELECTION_CHANGED` exists in the enum, but nothing in the knowledge base confirms a *readable* timeline selection. Note this is a different question from the known-impossible one — that was about *writing* the Project panel selection. | `SequenceEvent.SELECTION_CHANGED` + an unverified `sequence.getSelection()` | 10-min probe, then ½ day | ⚠️ **Probe before promising** |
| 3.5 | **Retime and transition badges** | Which of the nine uses is slowed, reversed, or has a transition on it. Partly done (speed, disabled). | `getSpeed()`, `isSpeedReversed()` ✅; transitions via `getComponentChain()` | ½ day | ⚠️ Effects-per-instance was ruled out of v1 — keep it to badges, not a list |
| 3.6 | **Persist the index** | Cache to the plugin's own data folder keyed on `project.path`, so reopening the panel on a big project is instant instead of a rescan. | `require("uxp").storage.localFileSystem.getDataFolder()` — no permission, no prompt | 1 day | ⚠️ Only if the live scan is slow. Measure first |
| 3.7 | **First-run tour** | Repo standing order: *a shipped feature nobody can find is not shipped*. "Select all N in this sequence" is a button people will not notice, and it is the whole pitch. | Port `Version Up/js/tour.js` | ½ day | 🔴 Required before release |
| 3.8 | **Group / sort toggle** | By sequence (now) vs by timecode vs by track. | UI only | 2 hours | ⚪ Nice, not needed |

---

## 4 · 🔴 Trace+. Do not build these in Trace.

Every one will feel like a small addition. Together they are the entire paid product, and the moment
Trace finds unused media, Trace+ has no reason to exist.

| Feature | Why it is Trace+ |
|---|---|
| **Find unused media** | The single most valuable thing in Trace+. Same engine, one line of code, and it gives away the pitch. |
| **Duplicate detection** | Project-wide aggregate. |
| **Empty bins / orphan sequences** | Project-wide aggregate. |
| **CSV or report export** | Sequence Index already exports one sequence; the project-wide report is Trace+'s deliverable. |
| **Cue-sheet totals across all clips** | "This music cue is used 7 times, 4m12s" for *every* clip at once. Genuinely unserved, genuinely worth money. |

The funnel only works if Trace teaches the need: after looking up a dozen clips one at a time,
"show me all 400 and move the unused ones" sells itself.

---

## 5 · Asked for, and not possible

Worth writing down so nobody re-researches it, and so the marketing copy never promises it.

| Wanted | Status | Evidence |
|---|---|---|
| Tint every usage orange in the timeline | ❌ | Colour labels are project-item only. No API for a clip already cut into a sequence. Selection and markers are the only two visual channels. |
| Click a timeline clip → reveal its source in the Project panel | ❌ | `ProjectUtils` is read-only. Nothing can select, scroll to, or highlight a Project panel item. |
| Zoom or scroll the timeline to frame a selection | ❌ | No API. Moving the playhead is the only navigation. §3.3 is the workaround. |
| A keyboard shortcut for next/previous | ❌ | Manifest `shortcut` key parses and does nothing in Premiere; Adobe staff, April 2026: "Not today, and no such work has been planned." Only in-panel `keydown` while the panel has focus. |
| Search across Productions or unopened projects | ❌ | The API almost certainly cannot reach unopened projects. This is the one real limitation people complain about — **do not promise it**. |
| Park the Source Monitor on the used frame | 🔴 26.3 | `SourceMonitor.setPosition()` is 26.3; the floor is 25.6. |
| Read a source clip's resolution | ❌ | No typed accessor on any of the four plausible classes. |

---

## 6 · The one strategic decision nobody has made

`manifest.json` declares **no network permission at all**. That is a real feature — fully offline is
what makes a free tool safe to install — but it costs three things VersionUp has:

- **No update check.** Every fix ships to nobody until they re-download.
- **No in-panel feedback form.** VersionUp's `report.js` is how bugs arrive.
- **No email capture in the panel.** The list gets built on the landing page instead.

For a lead magnet that is probably the right trade, and it should be a *decision* rather than an
accident. If it stands, the install guide has to carry an "check the page for updates" line, and the
tour has to carry the support email.

---

## What I would actually do, in order

1. **Load it.** Nothing on this page matters until §1 of `dev-install.md` has been walked and the scan
   time is a real number.
2. **§1.1–1.3**, the correctness set. A tool that says "0 uses" when there are nine is worse than no
   tool.
3. **§2.1 Mark all** and **§3.1 the coverage bar** — one for the demo GIF, one because nothing else
   does it.
4. **§3.7 the tour**, because the headline button is invisible without it.
5. Everything else only if someone asks for it twice.

# Trace — the launch list

Written 12 Aug 2026 against **0.14.1**. Supersedes the open items in `checklist.md`.

**Where it stands: the code is essentially done. The launch has not started.**
`trace-go-no-go.md` put the plugin at ~40% of the job. Sections 3 and 4 below are the other 60%.

---

## 0 · 🔴 Two things that will ship BROKEN unless done first

Neither of these fails loudly. Both are promises the panel already makes to the user.

- [ ] **Create the `trace-updates` GitHub repo and publish `dist/updates.json`.** The update check
      points at `raw.githubusercontent.com/nkcourses/trace-updates/main/updates.json`. Until that file
      exists, the check silently finds nothing forever — the code is fine, the promise is empty.
- [ ] **Put up `nkcourses.co.uk/trace`.** The ☰ → *Get help* item opens it, and the update notice's
      *What's new* button falls back to it. Shipping with both pointing at a 404 is worse than not
      having them.

---

## 1 · Verify in Premiere — an hour, and it is the cheapest hour left

Everything here is a real risk that only a running Premiere can settle.

- [ ] 🔴 **Three dock shapes.** Tall narrow strip · short wide bar under the timeline · small floating
      square. **Never tested.** Every overlap bug in VersionUp appeared only at small dock sizes and
      survived every check at a comfortable one.
- [ ] 🔴 **Does `altKey` actually arrive?** Alt is now the *only* modifier for locate — cmd, ctrl and
      shift all went to picking. If alt never reaches the panel, locate is unreachable. **Alt-click
      the ☰ and read the `modifiers seen` line.** If "alt" is not in it after trying, locate needs a
      different gesture.
- [ ] **The playhead lands on the exact frame.** If it is short by exactly the sequence start
      timecode, `getStartTime()` is relative and `setPlayerPosition()` is absolute.
- [ ] **Add markers to used** — never run against a real sequence. The marker type constant is a guess
      with a string fallback, and it is the only thing in the panel that writes.
- [ ] **No project open · project closed while the panel is watching · a 2,000-clip project.**
- [ ] **Merged and multicam clips** — currently *flagged in the caveat line*, not resolved. Decide
      whether that is honest enough for v1. It probably is: it says so rather than undercounting.

## 2 · Discoverability — the one real UX gap

- [ ] 🔴 **First-run tour.** There are now four gestures on a row — click, double-click, cmd-click,
      alt-click — plus tick boxes and two toggles. The status-line hint carries two of them. Standing
      order: *a shipped feature nobody can find is not shipped*, and right now "select all N" and
      "locate" are both invisible. Port `Version Up/js/tour.js`. **This is the last code task.**

## 3 · Package and publish

- [ ] Wire `brand/icons/` into `manifest.json` — the PNGs exist and the block is deliberately off.
      The panel is known to load now, so the reason for holding off is gone.
- [ ] Bump to **1.0.0**, confirm every `?v=` matches (smoke enforces it), run both suites.
- [ ] UDT → ••• → **Package** → `Trace.ccx`.
- [ ] Install on a *clean* machine from the .ccx, not from UDT. That is the only way to find out what
      a first-time user actually sees.
- [ ] Re-render the demo GIF **on the Mac** so it picks up Lato: `python3 brand/make-demo.py`.
- [ ] Outline the wordmark (`brand/trace-wordmark*.svg` are live text, not paths).

## 4 · The page, and the only number that matters

- [ ] **Landing page**: the GIF, the one-liner, an email field. Free on release.
      🔴 **This is the only step that measures the actual goal.** Trace exists to collect email
      addresses, so count email addresses — against a number set *before* you can move it.
- [ ] Install guide PDF + screenshots (`Version Up/brand/make-install-pdf.py`).
- [ ] Store copy · licence page · terms. They do not get shorter because the price is zero.
- [ ] Launch email to the VersionUp list.
- [ ] The **Video Usage tutorial video** — "here's the native way, and here's the free panel I built
      because the native way is painful" is a better video than either half, and it is free list
      growth from research already paid for.

## 5 · Validation that still has not run

Cheaper now than when it was written, because the GIF exists.

- [ ] Post the GIF on the open Adobe idea thread **"Better access to clip usage"** — 12 people already
      asked for exactly this.
- [ ] r/editors · r/premiere · one editors' Facebook group.
- [ ] Add "what would you want automated next?" to the VersionUp buyer email.

---

## The one-liner, unchanged, and the thing every reply will test

> **Select every use of a clip at once, across every sequence — without turning on a hidden column.**

Never *"Premiere can't find your clips"*. It can. The Video Usage column does it and Sequence Index
does the within-one-sequence version. What native cannot do is select every instance simultaneously,
and that is the whole pitch and the whole GIF.

## What NOT to add before launch

Frame the use · marker colours · multi-clip · retime badges · sort toggle · text-size control ·
anything with a settings sheet. All of it is either cut or Trace+. See `trace-vs-trace-plus.md`.

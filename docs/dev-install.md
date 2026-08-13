# Loading Trace in Premiere (development)

No build step. The folder on disk *is* the plugin. Same flow as VersionUp and Jumpstart Pro — if you
have loaded either of those, this is identical with a different folder.

## One-time setup

1. Install the **UXP Developer Tool (UDT)** from the Creative Cloud desktop app
   (Marketplace → Plugins → search "UXP Developer Tool"). It is free.
2. 🔴 **Turn on developer mode in Premiere.** Premiere Pro → **Settings → Plugins** (macOS;
   Edit → Preferences → Plugins on Windows) → tick **Enable developer mode** → **restart Premiere**.
   Off by default. Without it Premiere never registers with the UDT service and every load fails with
   "No applications are connected to the service", no matter how correct the plugin is. This is a
   prerequisite, not a troubleshooting step.
3. Turn on developer mode in UDT too — gear icon, bottom left. Adobe requires it in both.
4. Open Premiere Pro **25.6 or later** with a project open.
5. Open UDT. Premiere should now appear as connected.

## Load the plugin

1. UDT → **Add Plugin** → select **this folder's** `manifest.json` (`Trace/manifest.json`).
2. Click **Load**.
3. In Premiere: **Window → Extensions → Trace**.

VersionUp, Jumpstart Pro and Trace can all be loaded at once — different plugin ids, no collision.

## The edit loop

Use **Load & Watch** rather than Load, and keep **Debug** (the ••• menu) open. Every log line is
tagged `[Trace]`, and most failures show up in the console at load time rather than when you click
something.

⚠️ **Changes to `manifest.json` need Unload then Load**, not Reload. A reload does not re-read
permissions, so a corrected manifest still behaves like a broken one.

## 🔴 Trace is read-only — but still test on a duplicate

Trace makes no transactions. It never creates, renames, moves or deletes anything; the only things it
writes are the timeline **selection**, the **playhead position** and the **active sequence**. There is
nothing to undo.

That said, the standing order stands: open a **duplicate** of a real, messy past project. Both because
the rule exists for a reason, and because the interesting bugs only show up on a project with nests,
archived versions and a few hundred clips in it.

## First run checklist

Work through this in order — it is also the answer sheet for the open probes in `../CLAUDE.md`.

1. **Panel appears** and the status bar reads `Ready · click a clip in the Project panel`.
   White/blank instead means a JS error before the DOM wired up. Open Debug and read from the load.
2. **Click a clip in the Project panel.** The status should go to `Scanning 1/N …` with a live count,
   then to `N sequences · M clips indexed in Xms`.
   🔴 **Write that number down.** The acceptance test is under 5 seconds on a 30-sequence project.
3. **Click a second clip.** It must be instant — the index is cached. If it re-scans, the cache key
   is wrong and that is a bug, not a slow computer.
4. **Click a clip you know is used nowhere.** It should say "Used nowhere", calmly.
5. **Click a row.** Premiere should switch to that sequence, move the playhead to that frame, and
   select that clip.
   ⚠️ If the playhead lands short by exactly the sequence's start timecode, `getStartTime()` is
   relative and `setPlayerPosition()` is absolute — record it and fix the offset in `main.js`.
6. **Press the ▶ arrow repeatedly.** It should walk every use in panel order and wrap around.
7. **Find a clip used more than once in one sequence** and press **Select all N in this sequence**.
   All N should light up in the timeline at once, as a real selection you can nudge. **This is the
   demo GIF.** If it works, shoot it before doing anything else.
8. **Find a clip that only lives inside a nest.** It should show both the nest and the master, with
   the master's row labelled `via <nest name>`. Check the timecode by hand — click it and see whether
   the playhead lands on the actual clip.
9. **Select a bin.** It should say "That is a bin", not fall back to something confusing.
10. **☰ → Show scan report.** Paste the whole thing into the next session — it answers probes 1, 2
    and 3 in `../CLAUDE.md`, and those answers belong in `../../docs/premiere-uxp-api.md`.

## If the panel does not appear

- **`Plugin Load Failed` + `No applications are connected to the service`** — developer mode is off in
  Premiere, or Premiere is not running with a project open. This error is never about the plugin.
- **`Error reading manifest: ENOENT ... open '<old path>/manifest.json'`** — UDT stores the absolute
  path at Add time and does not follow the folder. Remove the plugin (`•••` → Remove Plugin) and
  re-add it at the new path.
- **Not in the Extensions menu at all** — the manifest failed to parse. Trailing commas, usually.
- **Appears but blank** — a JavaScript error before boot. Debug → console, from the moment of load.
- **Appears but nothing responds to a click** — the `entrypoints.setup()` failure mode. Trace never
  calls it and `test/smoke.js` fails if it appears, so this should be impossible; if it happens
  anyway, that is a new finding and belongs in the knowledge base.
- **Icons error** — `manifest.json` declares no icons on purpose. A declared icon path that does not
  exist on disk can stop the plugin loading. Only add an `icons` block once real PNGs are in `icons/`.

## Before every commit

```
node test/smoke.js && node test/usage-test.js
```

`smoke.js` is the repo's UXP rules turned into a build failure — no `gap`, no grid, no `transition`,
no `url(#…)`, no plain `<button>`, no `entrypoints.setup()`, every SVG shape carrying its own paint,
every `?v=` matching the manifest version. `usage-test.js` is the nest arithmetic, which is the one
place a plausible-looking wrong answer can hide.

## Packaging (later)

UDT → ••• → **Package** produces the `.ccx`. Bump `version` in `manifest.json` first, and bump the
`?v=` query on every `<script>` and `<link>` in `index.html` to match — `smoke.js` enforces it.

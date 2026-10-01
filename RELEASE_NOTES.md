## AleksMouseTester v0.2.15 — Delta View & clearer scores

**Honesty release. No change to the score formula. Existing sessions remain
fully compatible.**

This release shows more of what was measured and says more plainly why a
number is what it is: a new Delta View tab, a main graph that keeps every
spike, a Compare recommendation built only from comparable runs, and a score
breakdown that names the cap holding a score down. The first quick test after
starting the app now starts clean, too. If your scores differ from before, it
is not the formula.

### New

- **Delta View tab.** Shows the last completed or loaded run report by report:
  mouse movement (dX/dY) and the interval between reports on one shared,
  zoomable time axis. Reports that arrived buffered or with a shared timestamp
  are marked. The tab states whether the run is valid (red banner for an
  invalid run, amber for read errors) and says "full captured reports" only
  when no read errors or corrupt records occurred. It shows what arrived and
  when — it cannot tell a pause of your hand from a stall on the PC. It is
  disabled while a test runs and only does its work when you open it.
- **The app names the cap that holds a score.** When the delivery number sits
  exactly at one of the scoring caps, the score breakdown says so under
  *Delivery quality* — for example "limited by timing reliability (max batch
  5)" — and the Delivery tooltip in Compare explains that the number shown is
  that cap's value, not the weighted average of the metrics. A score that
  looks oddly round now tells you which single finding put it there.
- **More context in saved runs.** Session files now also record whether VBS
  and Memory Integrity are configured, whether Energy Saver was on, battery
  or AC power, and the process and OS architecture (see EXPORT_SCHEMA.md).
  None of these feed the score.

### Changed

- **Compare recommends only from comparable runs.** The sweet-spot
  recommendation used to pool every scorable run. It now uses only runs
  recorded with the currently selected mouse, DPI, USB port, power plan,
  Windows version, CPU, test protocol and duration. Other runs stay listed
  but dimmed, and the summary says how many runs were compared, how many were
  left out and why. Monthly Windows updates don't split your history; a
  feature update (for example 25H2 to 26H2) starts a new comparison group. If
  there isn't enough clean evidence, Compare says so instead of guessing.
- **Runs this version can't re-score stay untouched.** A run saved by a newer
  version, or by an old one without the needed metrics, is shown as Legacy with
  the reason — in Basic, Compare, Delta View and the exported report — instead
  of being re-scored under this version's formula.

### Fixed

- **The first quick test after starting the app starts clean.** It used to
  begin with one burst of buffered reports, mostly at 8000 Hz, because the app
  set up its measurement buffer at exactly that moment; the score then showed
  "limited by timing reliability". The buffer is now prepared when the app
  starts, and every quick test begins with a short warm-up that is discarded
  before the measured window — so a test takes half a second longer.
- **The main graph no longer hides spikes.** At high polling rates the graph
  drew only every Nth point, so a single long gap — or a single invalid report
  among tens of thousands — could vanish from the picture. Each pixel column
  now draws its highest and lowest value and any invalid report. Loading a run
  or switching a preset resets the zoom, so the "▲ N above …" note is never
  silently off.
- **The Timing Distribution histogram shows its peak again.** The outlier
  clamp added in v0.2.14 for line graphs was also applied to the histogram and
  cut almost every distribution off at 8 counts — the peak, i.e. the picture
  itself. Line graphs keep the clamp; the histogram no longer has it.
- **No stale numbers after Clear, cancel or a failed test.** Clear, a manual
  start, a cancelled and a failed test now reset every frozen display together
  — score, breakdown, headline, status, tiles and Delta View. After a cancel
  or failure the partial data is discarded instead of showing up as live
  numbers on Advanced.
- **Lost input data invalidates a run.** If reading raw input from Windows
  fails during a test, or a record arrives corrupted, the run is marked
  invalid instead of being scored as if nothing was lost.
- **Saved runs keep their own identity.** Exports use the finished run's mouse,
  labels, protocol and timestamps — typing into the fields afterwards no
  longer renames an old measurement. Viewing a saved run shows "Loaded <time>
  · <duration>" with that run's own verdict instead of the previous run's
  status line.
- **The diagnosis agrees with the score.** Diagnosis and recommendation are
  built from the same findings as the score, so a Limited run no longer gets a
  contradictory "stable" statement.
- **The reported rate is read safely.** The "N Hz reported" value is read with
  size and type checks and shows 0 (unknown) when Windows doesn't provide it,
  instead of an undefined value.
- **Compare's summary fits its text.** The summary above the run table now
  grows with its text instead of sitting in a fixed-height row, so it is no
  longer cut off on narrow windows or with display scaling.
- **The power tweak checks before it writes.** Every planned change is checked
  against the restore point before the first write; if any original value is
  missing, nothing is changed.

### Improved

- **A running test is protected.** Clear, device changes and restarts are
  blocked until the test has ended, and a cancelled or too-short run is never
  saved as a complete one.
- **A stopped manual capture says so.** The header shows that the capture has
  ended, how many reports it holds and how long it ran, and whether any reports
  were lost.
- **Windows PowerShell is started from its fixed system location.** The
  per-mouse USB power-saving switch no longer looks PowerShell up on the
  search path; if it is missing, nothing is changed and the state shows as
  unknown.
- **README:** clearer notes on SmartScreen, Smart App Control, which parts of
  the power tweak ask for administrator rights, and supported platforms.

### Unchanged on purpose

- Scoring formula, thresholds and rating classes
- Session format (schema 4, plus the optional context fields and the recorded
  warm-up length) and CSV export (schema 3). The CSV column `drain_errors` now
  also counts native read failures — see EXPORT_SCHEMA.md.
- Windows 10 22H2 / Windows 11, x64, self-contained (the .NET 8 runtime is
  included). Microsoft ends .NET 8 support on 10 November 2026; the move to
  .NET 10 will be its own release.
- Still unsigned (SmartScreen may warn — verify the SHA-256 on the release page)

Verify the download: the zip's SHA-256 is on this release page; inside the zip,
`SHA256SUMS.txt` lets you check every file (`sha256sum -c SHA256SUMS.txt`), and
`BUILDINFO.txt` now also names the .NET SDK and the bundled runtime version.

## AleksMouseTester v0.2.14 — stability & integrity

**Reliability release. No scoring changes. No file-format changes. Existing
sessions remain fully compatible.**

Every confirmed defect from a full audit of the published version is fixed in
this release — twenty-one in total, each verified in isolation. If your scores
differ from before, it is not this release — delivery-score-v3 is unchanged
(only the score *display* got more honest: invalid or limited runs can no
longer look better than they are).

### Fixed

- **Flickering numbers are gone.** Values no longer visibly blink — neither in
  idle (the UI previously recomputed and redrew everything every 400 ms even
  when nothing changed) nor during a running test (labels and their containers
  now paint double-buffered instead of erase-then-redraw).
- **Automatic cleanup can no longer lose a run's evidence.** If a session file
  could not be deleted (locked by antivirus/backup), its metrics CSV and raw
  data used to be deleted anyway while the run stayed visible. Now the whole
  set is preserved together, and the "deleted N oldest sessions" log line
  counts only what was actually deleted.
- **Leftover files clean themselves up.** Orphaned metrics/raw files of
  previously deleted sessions are now removed automatically — under
  deliberately conservative rules (only file names the app itself generates,
  never cloud-placeholder files, and only after being seen orphaned twice at
  least an hour apart), so user-created files in the same folder are never
  touched.
- **"Clear history" is honest about partial failure.** If some session files
  could not be deleted, you now get a clear message and they stay visible in
  Compare — previously they silently reappeared.
- **The system-context probe can no longer overlap a measurement.** Device and
  power queries now always complete before the measurement window opens.
  Probes skipped during a capture are retried afterwards, so a saved run can
  never carry the previous device's USB-port context.
- **Exported reports are internally consistent.** One decimal notation per
  document regardless of your Windows locale; a measured zero renders as a
  zero instead of an empty cell; the report's diagnosis line carries the same
  second-device note the UI shows; a recomputed score is labeled
  "(recomputed)" instead of sitting under the stored version line; and
  exporting after "Clear" can no longer wrap old data in a fresh session.
- **The score display tells one truth everywhere.** Invalid runs present
  identically in every view (no more red "Rejected" from a sentinel value),
  Compare tooltips name the run's actual problem and what to do about it, a
  "Limited" verdict never renders green, category tiles say "unknown" instead
  of guessing when their data is missing, and the category tiles now use the
  same green threshold as the score breakdown.
- **The power-saving check is safer and more honest.** Applying on a different
  power plan than the saved restore point now refuses instead of tweaking
  without a way back; a partially failed apply says exactly what was changed
  and how to undo it; an unreadable restore point blocks changes instead of
  being silently ignored; the elevated helper's timeout message no longer
  implies nothing happened; and a restore that switches your power plan back
  says so.
- **Housekeeping.** Leftover temporary/backup files from interrupted writes
  are cleaned up under the same conservative rules as orphaned session files.
- **Scaled displays now work properly.** On high-DPI screens (e.g. 1600p
  laptops or WQHD at 125-200% Windows scaling) the app used to fall apart:
  score-breakdown bars painted over the value numbers, the Advanced graph was
  squeezed to a stripe, buttons and panels kept their 96-DPI pixel sizes, and
  labels were cut off mid-word. The whole UI now scales with your display:
  pages scroll when vertical space runs out instead of crushing content, the
  graph keeps a guaranteed minimum height, tooltips appear only over the
  metric they belong to instead of over empty space, and when horizontal
  space runs out a bar steps aside before a number ever becomes unreadable.
  Verified by eye at 100% and at 150% on WQHD; the same rules apply at every
  other factor — if anything still looks wrong on your setup, please report
  it. (Thanks to the community report that caught this.)
- **The default graph axis no longer hides your data.** A single extreme
  outlier used to stretch the Y axis until the actual curve sat flat on the
  floor — the batch-size curve could disappear entirely. The default view now
  sticks to the typical band and says what it clipped ("▲ N above X — see
  Worst gap / Largest batch"); the clipped values stay visible as the Worst
  gap / Largest batch numbers in the metric band, and the wheel over the
  graph zooms without scrolling the page underneath.

### Improved

- **Graph navigation is discoverable.** The plot itself now tells you
  "Double-click → full screen", and full screen shows "Esc → back" — the
  gestures existed before, but nothing hinted at them.

### Unchanged on purpose

- Scoring formula, thresholds and rating classes (delivery-score-v3)
- Session and CSV export formats
- Still unsigned (SmartScreen may warn — verify the SHA-256 on the release page)

Verify the download: the zip's SHA-256 is on this release page; inside the zip,
`SHA256SUMS.txt` lets you check every file (`sha256sum -c SHA256SUMS.txt`).

## AleksMouseTester v0.2.13 — trust-hardening update

The scoring formula is unchanged (`delivery-score-v3`). This update is about trust: every number the app shows or exports is now provably the same number, with its validity attached — and the download is smaller and easier to verify.

### What's new
- **One score, everywhere** — the result view, your **Compare** history, and the exported report now all read the score through one path. An invalid or legacy run shows **"—"** in every one of them; a stored verdict can't be overridden by a lucky recompute.
- **Honest report export** — exporting a run you loaded from history now uses the run's **original stored data and score**. Heads-up: re-exporting an *old* run can show a **lower — true — score** than an export made with an earlier version (the old export path silently dropped the worst-case tail metrics and scored without them).
- **Two mice = no verdict** — if a second device contributed to a capture (e.g. a touchpad twitch), the run is marked **invalid (multiple devices)** instead of silently mixing two devices into one measurement. Timing intervals are always built per device.
- **Nothing is lost silently** — if a run can't be saved (OneDrive / Controlled Folder Access / full disk), the app says so and offers a retry. Unreadable history files are counted in Compare instead of vanishing. History keeps the most recent **300 runs** — that rule is now shown, and pruning is logged.
- **Crash diagnostics** — unhandled errors are written to a small rotating log (`%LocalAppData%\MouseTester\logs`), so a problem report can actually be diagnosed.
- **Live headline** — during a running test the score card now reads **"Live / Measuring"** instead of a stale "Ready".
- **Smaller, cleaner download** — the internal kernel-tracing machinery (~6 MB of native DLLs) is no longer shipped in the public build. `BUILDINFO.txt` states `build_flavor=consumer`.
- **Better CSV export** — every completed Quick Test writes a `_metrics.csv` (schema 3) next to its saved session: tail metrics (p95 / p99.9 / max gap / stutter & burst counts) plus the delivery score **with** its validity status and algorithm version. Format documented in the repo (`EXPORT_SCHEMA.md`).
- **Power tweak safety** — the USB power settings helper now takes an immutable restore point on first use, verifies every restore by reading the values back, and covers both AC and battery rails.

Your saved runs and settings carry over unchanged.

### Download & integrity
- `AleksMouseTester-v0.2.13-win-x64.zip` — Windows 10/11 x64, self-contained (no .NET install needed).
- The **SHA-256 of the `.zip`** is published on the GitHub release page — verify your download against it there.
- After unzipping, `SHA256SUMS.txt` lists the SHA-256 of every individual file — now in the standard format, so `sha256sum -c SHA256SUMS.txt` works out of the box (note: the file format changed vs earlier releases). `THIRD-PARTY-NOTICES.txt` lists the bundled open-source components.
- `BUILDINFO.txt` records the source commit, build flavor, and the pinned obfuscation tool version.

This release is a **folder** (the `.exe` plus its files), not a single `.exe`. **Unzip and keep the whole folder together**, then run `AleksMouseTester.exe`.

> Not code-signed yet — Windows SmartScreen may warn on first run ("More info" → "Run anyway").

### Honest status
Internally credible, **not yet validated across many mice / PCs**. Feedback from different hardware is very welcome.

*Reports Windows host raw-input delivery timing, not physical USB/HID polling timing.*

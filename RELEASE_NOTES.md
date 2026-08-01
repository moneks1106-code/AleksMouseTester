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

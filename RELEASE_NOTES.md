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

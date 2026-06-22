# MouseTester

**A free Windows tool that measures how _evenly_ your mouse's input actually arrives at your PC — and is honest about what that means.**

Most mouse testers show you a polling-rate number. MouseTester measures something different and more useful: the **timing of how raw mouse input is delivered to the Windows host** — how steady the intervals are, how often there are gaps or stutters, and which of *your* settings actually deliver cleanly on *your* machine.

> **Honest by design.** MouseTester reports **Windows host raw-input delivery timing — not** physical USB/HID polling rate, **not** click-to-photon latency, **not** a verdict on your hardware. Results are correlations on your own PC, not proof of causation. When a measurement isn't trustworthy, the tool says so instead of glowing green.

## What it does

- **Quick Test** — move the mouse for a few seconds, get a plain-language delivery score.
- **Advanced / Lab** — the full metric breakdown (jitter, worst gap, stutters, batching, on-target share…), a live interval graph, and the raw numbers behind the score.
- **Compare** — every run is saved locally and ranked, so you can find your **sweet spot: the highest rate that still delivers cleanly** (higher Hz isn't automatically better). Testing several USB ports? Compare them side by side.
- **Power-saving check** — optionally scan the Windows power-saving settings that can interrupt input delivery (USB selective suspend, CPU states, per-device USB power management) and switch them to max performance — fully reversible.

## The core idea

> It doesn't reward the highest nominal polling rate. It rewards the highest rate that stays **stable** under real host delivery.

An 8000 Hz mouse that delivers raggedly can score *below* a rock-steady 4000 Hz run — and MouseTester shows you why, with the numbers.

## Download & run

1. Grab the latest `MouseTester-vX.Y.Z-win-x64.zip` from [**Releases**](../../releases).
2. Unzip (keep the whole folder) and run **`MouseTester.exe`**.
3. **Windows 10/11, 64-bit.** Self-contained — no .NET install needed.

Verify your download against the `SHA256` value listed on the release.

> The build is **not code-signed yet**, so Windows SmartScreen may warn on first run ("More info" → "Run anyway"). Signing is planned.

## Privacy & what it touches

- Reads **raw mouse input locally** to measure timing. **No network, no telemetry — nothing leaves your PC.** Test history is stored only in your Documents folder.
- The optional power-saving changes require **admin** (a UAC prompt), are shown to you first, and are **reversible** ("Restore settings"). Nothing is changed without you clicking.

## Status

Early, pre-1.0. **Internally credible, but not yet validated across many mice / PCs** — that's the next step. Feedback and measurements from different hardware are very welcome.

## License

Free to use. Binary-only release — see [LICENSE](LICENSE). Not open source.

---

*MouseTester is a diagnostic instrument for host input-delivery timing — not a marketing-number generator.*

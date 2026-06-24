## MouseTester v0.2.10

A fix-focused release — mostly things that keep the tool *honest about itself*.

### What's fixed
- **The "sweet spot" recommendation is now honest.** Compare ranks each rate by its **typical** (median) delivery across your runs, so it points you to the rate that's *consistently* clean — not the highest rate that happened to nail it once. On a system where 8000 Hz is sometimes great but often stutters, it now correctly recommends the steadier rate (the whole point: the highest **stable** rate, not the highest number).
- **Per-mouse USB power saving now reads the real state.** The "Check power saving" panel used to show a stale **"On"** for your mouse even after you disabled it (a custom power plan could trigger this). It now reads the exact same Windows state it writes, **verifies** the change actually took effect (no more false "done"), refreshes the panel immediately, and shows **"Mixed"** when a composite mouse's interfaces disagree.
- **Reopened runs no longer hide their worst spike.** Double-clicking a saved run on the Compare page now always shows its **worst gap and worst batch** in the graphs — the earlier downsampling could drop the single biggest outlier, and a saved graph should never look cleaner than the real run.
- **Power tweaks are truly reversible.** The optional power tweak now changes **nothing** if it can't save a restore point first, so "Restore settings" always has something to restore.
- **The tool stays out of its own measurement.** Power-saving actions are blocked while a test or capture is running, so checking/changing power settings can never disturb a live reading.

---

A free Windows tool that measures how **evenly** your mouse's input is delivered to the host — honest that this is host-delivery *timing*, not a device polling-rate or latency claim.

### Highlights
- **Quick Test** with a plain-language delivery score
- **Advanced / Lab** metrics + a live interval graph (double-click it for a labeled full-screen view)
- **Compare** your runs, find your **stable sweet-spot rate** (higher Hz isn't automatically better), and **double-click any run to reopen it** — values, ratings, and graphs
- Optional **power-saving check / disable / restore** (reversible, admin-gated)

### Download
- `MouseTester-v0.2.10-win-x64.zip` — Windows 10/11 x64, self-contained (no .NET install needed)
- SHA256: `d499e35e50596f2ac0e0e22bb3449e199b0683097f39b7aeb09b61a54d52e3c1`

Unzip (keep the whole folder) and run `MouseTester.exe`.

> Not code-signed yet — Windows SmartScreen may warn on first run ("More info" → "Run anyway"). Signing is planned. See **How this was built** in the README for AI-assistance, closed-source status, and what the SHA-256 does and doesn't prove.

### Honest status
Internally credible, **not yet validated across many mice / PCs**. Feedback from different hardware is very welcome.

*Reports Windows host raw-input delivery timing, not physical USB/HID polling timing.*

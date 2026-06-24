## MouseTester v0.2.9

### What's new in this version
- **Open any past run.** On the **Compare** page, **double-click a run** to load it back into the full UI — its values, ratings, score breakdown, metric band, and graphs. New runs store a compact trace so the **time-series graphs reload too**; older runs (recorded before this version) show the saved distribution graph.
- **Fix: correct reference line when switching runs.** The red target line on the Timing-Distribution graph used to keep the *previous* run's rate when you clicked between saved runs. It now always follows the loaded run's own target Hz.
- **Lighter on your system while measuring.** Reduced the app's own overhead during a capture — higher-priority capture thread, garbage-collection pauses suppressed for the measurement window, fewer lock reads, and no redundant post-capture passes. An honest measurement tool should disturb the very timing it measures as little as possible; this matters most at 8000 Hz.

---

A free Windows tool that measures how **evenly** your mouse's input is delivered to the host — honest that this is host-delivery *timing*, not a device polling-rate or latency claim.

### Highlights
- **Quick Test** with a plain-language delivery score
- **Advanced / Lab** metrics + a live interval graph (double-click it for a labeled full-screen view)
- **Compare** your runs, find your **stable sweet-spot rate** (higher Hz isn't automatically better), and **double-click any run to reopen it** — values, ratings, and graphs
- Optional **power-saving check / disable / restore** (reversible, admin-gated)

### Download
- `MouseTester-v0.2.9-win-x64.zip` — Windows 10/11 x64, self-contained (no .NET install needed)
- SHA256: `a0b48d6b3f2a83efac4ce8d1cee9ca454e41ba0d8b7d80d4157a9844d4ff0210`

Unzip (keep the whole folder) and run `MouseTester.exe`.

> Not code-signed yet — Windows SmartScreen may warn on first run ("More info" → "Run anyway"). Signing is planned.

### Honest status
Internally credible, **not yet validated across many mice / PCs**. Feedback from different hardware is very welcome.

*Reports Windows host raw-input delivery timing, not physical USB/HID polling timing.*

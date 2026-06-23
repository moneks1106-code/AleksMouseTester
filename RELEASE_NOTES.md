## MouseTester v0.2.8

### What's new in this version
- **Better across monitor resolutions.** The app now opens **maximized**, so the graph has real room on smaller / Full-HD (1080p) screens — the earlier default window left it cramped.
- **Full-screen graph.** **Double-click** any graph to open it full screen. It keeps the scroll-wheel **zoom** and adds dropdowns to switch the graph / X-axis. Press **Esc** (or double-click again) to close it.
- **Labeled axes** in the full-screen graph — titled X and Y axes with the exact value ticks (there's finally room for them at full size).

---

A free Windows tool that measures how **evenly** your mouse's input is delivered to the host — honest that this is host-delivery *timing*, not a device polling-rate or latency claim.

### Highlights
- **Quick Test** with a plain-language delivery score
- **Advanced / Lab** metrics + a live interval graph (double-click it for a labeled full-screen view)
- **Compare** your runs and find your **stable sweet-spot rate** (higher Hz isn't automatically better)
- Optional **power-saving check / disable / restore** (reversible, admin-gated)

### Download
- `MouseTester-v0.2.8-win-x64.zip` — Windows 10/11 x64, self-contained (no .NET install needed)
- SHA256: `bd5ecb07c5a2db80a8f55708d45473db18d8fdd1468ab59713f6f78d7242aa27`

Unzip (keep the whole folder) and run `MouseTester.exe`.

> Not code-signed yet — Windows SmartScreen may warn on first run ("More info" → "Run anyway"). Signing is planned.

### Honest status
Internally credible, **not yet validated across many mice / PCs**. Feedback from different hardware is very welcome.

*Reports Windows host raw-input delivery timing, not physical USB/HID polling timing.*

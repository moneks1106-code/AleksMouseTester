## AleksMouseTester v0.2.11

### Renamed: MouseTester → **AleksMouseTester**
This project is now **AleksMouseTester** — an **independent** tool with its own focus: how *evenly* your mouse's input is actually delivered to the host (timing stability, honest validity flags, the highest *stable* rate as your sweet spot). The rename makes clear it is **not** the older, unrelated **MouseTester** by microe1 — and ends any "stealing the name" nonsense.

- **Purely a name change from v0.2.10** — the measurement, scoring and features are unchanged; only the visible name moved (window/dialogs, the executable, this repo).
- **Your saved history carries over.** The data folder is unchanged, so all your existing runs and settings are still there after updating.
- The download is now `AleksMouseTester.exe` inside `AleksMouseTester-v0.2.11-win-x64.zip`.

---

A free Windows tool that measures how **evenly** your mouse's input is delivered to the host — honest that this is host-delivery *timing*, not a device polling-rate or latency claim.

### Highlights
- **Quick Test** with a plain-language delivery score
- **Advanced / Lab** metrics + a live interval graph (double-click it for a labeled full-screen view)
- **Compare** your runs, find your **stable sweet-spot rate** (the rate that's *consistently* clean, not the highest one that managed it once), and **double-click any run to reopen it** — values, ratings, and graphs
- Optional **power-saving check / disable / restore** (reversible, admin-gated)

### Download
- `AleksMouseTester-v0.2.11-win-x64.zip` — Windows 10/11 x64, self-contained (no .NET install needed)
- SHA256: `b5f52377235100d302070f4c368d8af81abfbfadeb97d8095bf7d6021d40beed`

Unzip (keep the whole folder) and run `AleksMouseTester.exe`.

> Not code-signed yet — Windows SmartScreen may warn on first run ("More info" → "Run anyway"). See **How this was built** in the README for AI-assistance, closed-source status, and what the SHA-256 does and doesn't prove.

### Honest status
Internally credible, **not yet validated across many mice / PCs**. Feedback from different hardware is very welcome.

*Reports Windows host raw-input delivery timing, not physical USB/HID polling timing.*

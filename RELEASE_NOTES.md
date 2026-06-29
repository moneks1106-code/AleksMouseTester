## AleksMouseTester v0.2.12 — clarity & honesty update

The measurement, scoring and your saved history are unchanged. This update makes the results clearer and more honest.

### What's new
- **Clearer Quick Test** — one **Start Quick Test** button, a plain-language result, and the measurement scope shown right at the result: what it measures (how *evenly* your mouse's input arrives at the Windows raw-input boundary) and what it does **not** (USB polling rate, sensor or firmware latency).
- **Honest invalid runs** — a test with too few samples or a capture hiccup now shows **"—"** and *"couldn't be evaluated reliably"* instead of a misleading low score, in both the Quick Test result and your **Compare** history. (It could previously read as a scary "1.0".)
- **Metric hover help** — hover any **Advanced / Lab** metric for a plain-language explanation.
- **Less UI interference during capture** — reduced UI work while a test is running, so the app does less background drawing during measurement.

Your saved runs and settings carry over unchanged.

### Download & integrity
- `AleksMouseTester-v0.2.12-win-x64.zip` — Windows 10/11 x64, self-contained (no .NET install needed).
- **SHA-256 of the `.zip`:** `402bf83fde9f0d8540910170cf8f076eab44dc8d703fcddaaee3dcd62bec2f76`
- After unzipping, `SHA256SUMS.txt` (in this folder) lists the SHA-256 of every individual file, and `THIRD-PARTY-NOTICES.txt` lists the bundled open-source components.

This release is a **folder** (the `.exe` plus its files), not a single `.exe`. **Unzip and keep the whole folder together**, then run `AleksMouseTester.exe`.

> Not code-signed yet — Windows SmartScreen may warn on first run ("More info" → "Run anyway").

### Honest status
Internally credible, **not yet validated across many mice / PCs**. Feedback from different hardware is very welcome.

*Reports Windows host raw-input delivery timing, not physical USB/HID polling timing.*

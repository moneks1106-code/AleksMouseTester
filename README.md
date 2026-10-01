# AleksMouseTester

**A free Windows tool that measures how _evenly_ your mouse's input actually arrives at your PC — and is honest about what that means.**

> *AleksMouseTester is an independent tool — **not affiliated with** the older "MouseTester" desktop app by microe1. It was renamed (from "MouseTester") specifically to make that distinction clear and avoid any confusion.*

Most mouse testers show you a polling-rate number. AleksMouseTester measures something different and more useful: the **timing of how raw mouse input is delivered to the Windows host** — how steady the intervals are, how often there are gaps or stutters, and which of *your* settings actually deliver cleanly on *your* machine.

> **Honest by design.** AleksMouseTester reports **Windows host raw-input delivery timing — not** physical USB/HID polling rate, **not** click-to-photon latency, **not** a verdict on your hardware. Results are correlations on your own PC, not proof of causation. When a measurement isn't trustworthy, the tool says so instead of glowing green.

## What it does

- **Quick Test** — move the mouse for a few seconds, get a plain-language delivery score.
- **Advanced / Lab** — the full metric breakdown (jitter, worst gap, stutters, batching, on-target share…), a live interval graph (**double-click it for a full-screen view** with zoom + labeled axes; Esc closes), and the raw numbers behind the score.
- **Delta View** — the finished run's movement (relative X/Y mouse counts) and its delivery timing drawn on one shared timeline, so a timing gap can be read against the movement at the same moment. Jump straight to the longest gap, or zoom to a 400 ms / 100 ms window. An invalid run is labelled as such on the page instead of being shown as clean.
- **Compare** — every run is saved locally and ranked, so you can find your **sweet spot: the highest rate that still delivers cleanly** (higher Hz isn't automatically better). The recommendation is made only for the **cohort of your current selection** — same mouse model (VID/PID), DPI, USB port, power plan, Windows build, CPU, test protocol and duration — and Compare says how many runs are in that cohort, how many were excluded and **why** (different device, DPI, port, plan, build…, or context that was never recorded), and how many can't be scored. Excluded runs stay in the table, dimmed, with the reason in their tooltip. **Double-click any run to reopen it** — its values, ratings, and graphs. Testing several USB ports? Every run stays visible, so you can still look at them side by side — the recommendation just won't mix them.
- **Power-saving check** — optionally scan the Windows power-saving settings that can interrupt input delivery (USB selective suspend, processor min/max state, CPU core parking, PCIe link power management, plus the per-mouse USB power switch) and switch them to max performance — fully reversible: the first change takes an immutable restore point of your original values (AC **and** battery), and every restore is verified by reading the values back.

## The core idea

> It doesn't reward the highest nominal polling rate. It rewards the highest rate that stays **stable** under real host delivery.

An 8000 Hz mouse that delivers raggedly can score *below* a rock-steady 4000 Hz run — and AleksMouseTester shows you why, with the numbers.

## Download & run

1. Grab the latest `AleksMouseTester-vX.Y.Z-win-x64.zip` from [**Releases**](../../releases).
2. Unzip (keep the whole folder) and run **`AleksMouseTester.exe`**.
3. **Windows 10 22H2 / Windows 11, x64.** Self-contained — no .NET install needed. On Windows 11 on Arm devices the x64 build runs under Windows' x64 emulation — it has **not** been validated there. Note that Windows 10 reached Microsoft's end of support in October 2025.

Verify your download against the `SHA256` value listed on the release.

> On the first run of each release, Windows SmartScreen may say "Windows protected your PC" — because this is a small, free, **unsigned** tool, not because it's malware. Click **More info → Run anyway**. On a PC where **Smart App Control** is switched on there is no "Run anyway" — see [Is it safe?](#is-it-safe) below for both, and for how to verify the download yourself.

## Is it safe?

Short version: yes — and you don't have to take my word for it.

- **Why the SmartScreen warning?** AleksMouseTester is a free tool I don't earn anything from, and it is **not code-signed — by decision, for now** (code-signing certificates for individual developers do exist). **Unsigned ≠ unsafe** — it only means Windows has no verified publisher to attach a reputation to. SmartScreen tracks reputation per file for unsigned programs, so **every new release starts again at zero reputation** — expect the warning on each new version, not only the first.
- **Smart App Control (Windows 11).** On a PC where Smart App Control is in its *On* (enforcing) mode, Windows blocks an unsigned program without established reputation outright — there is no "More info → Run anyway". Expect an unsigned release that Windows does not know yet not to start there. I do **not** recommend turning Smart App Control off for this tool.
- **No network, no telemetry.** It reads raw mouse input **locally** to measure timing — **nothing leaves your PC**, no internet calls, no tracking. Test history is stored only in your Documents folder, plus a small **local** error log (`%LocalAppData%\MouseTester\logs`) so crashes can be diagnosed — it never leaves your machine either.
- **The optional power-saving changes** are shown to you first and are fully **reversible** ("Restore settings"). The power-plan tweak changes exactly **five** settings of the active Windows power plan — USB selective suspend, minimum and maximum processor state, CPU core parking, and PCIe link power management (ASPM) — on both the AC and the battery rail, and it runs **without** elevation: it writes them through the Windows power API without asking for admin, and if Windows refuses a write, the tool reports it instead of prompting for admin. Only the separate **per-mouse USB power switch** ("allow the computer to turn off this device to save power") needs admin and shows a UAC prompt. The restore point with your original values is written to your own user profile (`Documents\MouseTester\power_snapshot.json`) before the first change. Nothing changes unless you click.

**Don't trust me — verify:**
- Check the download's **SHA256** matches the release — it confirms you got my exact, untampered file (for what the hash *can't* prove, see [How this was built](#how-this-was-built-transparency)).
- After unzipping, verify **every individual file**: `sha256sum -c SHA256SUMS.txt` (standard format — works in git-bash/WSL out of the box).
- `BUILDINFO.txt` in the folder records the exact source commit, build flavor, and the pinned obfuscation-tool version the build used.
- Drag the `.zip` onto [**VirusTotal**](https://www.virustotal.com) to scan it with 70+ antivirus engines yourself.
- Block it in a firewall if you like — it never needs the internet.

## How this was built (transparency)

- **AI-assisted.** I designed and directed AleksMouseTester — the measurement methodology, the "honest instrument" rules, what to measure and how to score it — and wrote the code with heavy help from an AI coding assistant. The design and testing decisions are mine; much of the code is AI-written. Saying so plainly because you deserve to know.
- **Closed-source, by choice.** This repo is binary-only (README, license, notes, download). The source stays private for now — a deliberate choice, but honestly **it limits independent verification**: you can't read or rebuild the code.
- **What the SHA-256 does and doesn't prove.** It only confirms your download matches the file I uploaded — it does **not** prove the program is safe, and without source you can't diff it against reviewable code. Treat it as "you got the right, untampered bytes," not "it's been audited."
- **No git commit history here** because the repo is binary-only (the source history lives in a private repo). An empty-looking repo can read as sketchy — hence this section.

So the strongest checks you have are the ones under [Is it safe?](#is-it-safe) above — scan it on VirusTotal, run it offline/firewalled, and note it makes no network calls and has no telemetry. Prefer open source or a signed build? Fair — both are on the list as the project earns trust.

## Status

Early, pre-1.0. **Internally credible, but not yet validated across many mice / PCs** — that's the next step. Feedback and measurements from different hardware are very welcome.

## License

Free to use. Binary-only release — see [LICENSE](LICENSE). Not open source.

---

*AleksMouseTester is a diagnostic instrument for host input-delivery timing — not a marketing-number generator.*

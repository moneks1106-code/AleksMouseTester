# AleksMouseTester

**A free Windows tool that measures how _evenly_ your mouse's input actually arrives at your PC — and is honest about what that means.**

> *AleksMouseTester is an independent tool — **not affiliated with** the older "MouseTester" desktop app by microe1. It was renamed (from "MouseTester") specifically to make that distinction clear and avoid any confusion.*

Most mouse testers show you a polling-rate number. AleksMouseTester measures something different and more useful: the **timing of how raw mouse input is delivered to the Windows host** — how steady the intervals are, how often there are gaps or stutters, and which of *your* settings actually deliver cleanly on *your* machine.

> **Honest by design.** AleksMouseTester reports **Windows host raw-input delivery timing — not** physical USB/HID polling rate, **not** click-to-photon latency, **not** a verdict on your hardware. Results are correlations on your own PC, not proof of causation. When a measurement isn't trustworthy, the tool says so instead of glowing green.

## What it does

- **Quick Test** — move the mouse for a few seconds, get a plain-language delivery score.
- **Advanced / Lab** — the full metric breakdown (jitter, worst gap, stutters, batching, on-target share…), a live interval graph (**double-click it for a full-screen view** with zoom + labeled axes; Esc closes), and the raw numbers behind the score.
- **Compare** — every run is saved locally and ranked, so you can find your **sweet spot: the highest rate that still delivers cleanly** (higher Hz isn't automatically better). **Double-click any run to reopen it** — its values, ratings, and graphs. Testing several USB ports? Compare them side by side.
- **Power-saving check** — optionally scan the Windows power-saving settings that can interrupt input delivery (USB selective suspend, CPU states, per-device USB power management) and switch them to max performance — fully reversible: the first change takes an immutable restore point of your original values (AC **and** battery), and every restore is verified by reading the values back.

## The core idea

> It doesn't reward the highest nominal polling rate. It rewards the highest rate that stays **stable** under real host delivery.

An 8000 Hz mouse that delivers raggedly can score *below* a rock-steady 4000 Hz run — and AleksMouseTester shows you why, with the numbers.

## Download & run

1. Grab the latest `AleksMouseTester-vX.Y.Z-win-x64.zip` from [**Releases**](../../releases).
2. Unzip (keep the whole folder) and run **`AleksMouseTester.exe`**.
3. **Windows 10/11, 64-bit.** Self-contained — no .NET install needed.

Verify your download against the `SHA256` value listed on the release.

> On first run, Windows SmartScreen may say "Windows protected your PC" — because this is a small, free, **unsigned** tool, not because it's malware. Click **More info → Run anyway**. (See [Is it safe?](#is-it-safe) below for why, and how to verify it yourself.)

## Is it safe?

Short version: yes — and you don't have to take my word for it.

- **Why the SmartScreen warning?** AleksMouseTester is a free tool I don't earn anything from, so it isn't code-signed (a signing certificate costs money and needs a registered company — not worth it for a free project). **Unsigned ≠ unsafe** — it only means Windows doesn't recognise the publisher yet. The warning fades as more people run it.
- **No network, no telemetry.** It reads raw mouse input **locally** to measure timing — **nothing leaves your PC**, no internet calls, no tracking. Test history is stored only in your Documents folder, plus a small **local** error log (`%LocalAppData%\MouseTester\logs`) so crashes can be diagnosed — it never leaves your machine either.
- **The optional power-saving changes** require **admin** (a UAC prompt), are shown to you first, and are fully **reversible** ("Restore settings"). Nothing changes unless you click.

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

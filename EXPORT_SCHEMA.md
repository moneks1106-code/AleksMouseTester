# `_metrics.csv` — export schema

*Current: `schema_version` **3**. One header line, one data row per file. Numbers use the
invariant culture (`.` decimal separator) regardless of the OS locale. v2's columns keep their
exact positions; everything new in 3 is **appended**, so a v2-era parser that reads by index keeps
working.*

## Columns (schema 3)

| # | column | notes |
|---:|---|---|
| 1 | `schema_version` | `3` |
| 2 | `timing_claim` | always `host_delivery_not_device_polling` |
| 3 | `timestamp_basis` | `Stopwatch host timestamp` |
| 4 | `valid_dt_policy` | `exclude intra-batch and buffered intervals` |
| 5–8 | `protocol,duration_s,dpi,target_hz` | test identity |
| 9–14 | `T_nom_ms,f_nom_Hz,median_ms,IQR_ms,p01_ms,p99_ms` | core timing |
| 15–22 | `double_pct,outlier_pct,overall_ev_s,N,valid_dt,buffered_pct,max_batch,timing_valid` | rates / delivery integrity |
| 23–24 | `drain_errors,corrupt_headers` | capture integrity |
| 25–28 | `abs_counts,mov_reports,cts_s,mov_pct` | movement |
| 29–38 | `testEnvironment.profile,gameRunning,gameName,fpsCap,overlaysEnabled,powerProfile,usbPortNotes,pollingRateHz,backgroundLoadNotes,claimScope` | environment metadata block |
| 39 | `environment_interpretation` | profile hint |
| **40–44** | `p95_ms,p99_9_ms,max_gap_ms,gap_count,burst_count` | **new in 3** — tail-latency and gap metrics |
| **45** | `delivery_score` | **new in 3** — the 1–10 headline, **EMPTY unless the run is valid** (never a sentinel like `1.0`) |
| **46** | `score_status` | **new in 3** — `Valid` / `InvalidLowSamples` / `InvalidCaptureIntegrity` / `InvalidMultiDevice`; empty when no score report exists |
| **47** | `score_algorithm_version` | **new in 3** — e.g. `delivery-score-v3`; scores are comparable only between rows with the same value here |
| **48** | `invalid_reasons` | **new in 3** — pipe-joined (`CaptureIntegrity|LowSamples`), empty when valid |
| **49** | `device_event_counts` | **new in 3** — per-device event counts, semicolon-joined, descending (`79000;1026`). **EMPTY = unverified** (pre-device-isolation data) — never to be read as "1 device" |

## Semantics guarantees

- `delivery_score`, `score_status`, `invalid_reasons` are derived from the **same** central
  validity predicate the saved session JSON uses — the CSV and the saved session can never
  disagree on validity.
- A score value is never emitted for an invalid run; consumers must treat an empty
  `delivery_score` + non-`Valid` `score_status` as "not evaluable", not as zero.
- `drain_errors` retains its legacy column name and, since v0.2.15, counts native read failures
  from both the current `GetRawInputData` event and `GetRawInputBuffer` drains.
  `corrupt_headers` includes malformed current-event records. Successfully read
  non-mouse events are intentionally skipped and do not increment either counter.
  Either nonzero counter uses the existing capture-integrity validity gate; the
  score formula and schema version remain unchanged.

## Session JSON — `systemContext` inventory fields (added in v0.2.15, `schema_version` stays 4)

The saved session JSON (`systemContext`, captured on a background thread and never while a
measurement runs) gained seven **additive, nullable** members. Absent = not captured, never
"off"; every member carries a `Provenance` entry (`ReadOk` / `Unknown`), and
`AutoCaptureSchemaVersion` stays `1`. They are **inventory** for telling runs apart after the
fact — nothing in the score or its validity rules reads them, and Compare does not use them to
group runs; Energy Saver and battery state appear there only as notes.

| member | source | notes |
|---|---|---|
| `VbsConfigured` | `HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard\EnableVirtualizationBasedSecurity` | **configured**, not proven running |
| `HvciConfigured` | `…\DeviceGuard\Scenarios\HypervisorEnforcedCodeIntegrity\Enabled` | Memory Integrity as configured, not proven running |
| *(HvciRunning)* | — | **deliberately not captured**: reading the running state would need a WMI query or a helper process while probing, which the app avoids |
| `EnergySaverActive` | `SYSTEM_POWER_STATUS.SystemStatusFlag` bit 0 | state at probe time; can be on while on AC since 24H2 |
| `OnBattery` | `SYSTEM_POWER_STATUS.ACLineStatus` | `0` → `true`, `1` → `false`, `255` (unknown) → absent |
| `ProcessArchitecture` | `RuntimeInformation.ProcessArchitecture` | e.g. `X64` |
| `OsArchitecture` | `RuntimeInformation.OSArchitecture` | native machine, e.g. `Arm64` |
| `IsEmulated` | `ProcessArchitecture != OsArchitecture` | `true` = the x64 build ran under emulation on an ARM64 host |

The same seven values appear as rows in the reference-bundle Markdown report (empty cell = not
captured). They are **not** in `_metrics.csv`: by this document's own convention an appended column
is a schema bump (2 → 3 did exactly that), so the CSV stays at schema 3, byte-identical.

## Session JSON — run field `preRollMs` (added in v0.2.15, `schema_version` stays 4)

Each quick-test run in the session JSON carries `preRollMs`: the length of the **discarded warm-up**
that ran right before the scored window, in milliseconds (500 since v0.2.15). The capture already
runs during the warm-up, and everything captured in it is thrown away at one atomic boundary; the
run's duration, samples and metrics all start at that boundary. Absent on runs saved before v0.2.15,
which had no warm-up. Provenance only — nothing in the score or its validity rules reads it. It is
**not** a `_metrics.csv` column (no schema bump).

## Changelog
- **3, unchanged (v0.2.15)** — VBS/HVCI/Energy-Saver/architecture context added to the session JSON
  `systemContext` only (section above); no CSV column appended, no bump. `drain_errors` also counts
  native read failures (see *Semantics guarantees*). Runs record `preRollMs` in the session JSON.
- **3** — appended tail metrics, score + validity + algorithm-version provenance, and the raw
  per-device observation. (Before 3, the score existed only in the session JSON and the CSV carried
  no tail columns.)
- **2** — environment metadata block + interpretation hint.

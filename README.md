# audio-rate

**Acoustic exercise-exertion estimation from breath audio.** A phone or wired-headset microphone records breathing during cycle-ergometer exercise; a frozen log-mel front end and a small CNN + GRU regress continuous exertion as **%HRR** per 10-second window, validated against a Polar H10 chest strap (RR intervals → %HRR) on held-out participants and held-out handsets.

UIUC **SE 498 — AI-Driven Intelligent Systems**, Fall 2026, Prof. Yiwen Dong. Team 3 **Audio Rate**: Jude Welsch · Joshua Jimenez · Ray Chang.

> Reproducibility is graded: the final report (Sat 2026-12-12) must be reproducible from this repo. Raw audio, ECG and IMU recordings **never** enter it — see `docs/data-management-plan.md`.

## Layout

| Folder | Holds | Status |
|---|---|---|
| `capture/` | the iOS capture app — unprocessed 16 kHz audio, IMU on the audio clock, Polar H10 RR / ECG / ACC via the Polar BLE SDK, event marks, session bundle | scaffold only — build plan in `docs/capture-app-build-scaffold.md` |
| `features/` | the **frozen** log-mel front end (25 ms Hann · 10 ms hop · 64 mel · 16 kHz · fixed floor clamp), shared byte-for-byte between the app and the training pipeline; parity test | empty |
| `labels/` | RR intervals → artifact-rejected HR → %HRR (Karvonen, Tanaka HRmax); per-participant lag fit by envelope cross-correlation | empty |
| `model/` | CNN per 10 s window → GRU over 30–60 s → %HRR regression (Huber); IMU-only and power-only baselines; Core ML export | empty |
| `eval/` | session QA script (cal-tone linearity, room-tone floor, clap offsets, RR rejection, lag), LOPO / LODO splits, per-phase MAE, zone accuracy strict / ±1, Bland–Altman | empty |
| `data/` | the **released** per-window features + labels and the data card — only after the re-identification audit; never raw recordings | data card template only |
| `docs/` | the build scaffold, data-management plan, lab runbook and pipeline note (Markdown + PDF), exported from the project vault | current as of 2026-09-22 |

## Build order

Phase 0 instrument (`capture/` + qualification test + H10 ingestion + `features/` parity) → pilot session one → cohort (< 10, one session each) → offline QA + labels → training + evaluation → on-device build + live validation. Dates and go/no-go gates: `docs/pipeline-input-to-verdict.md` and the vault's Milestones.

## Setting up the build machine (Mac)

1. Xcode 15+ with an iOS 17 simulator and the three team iPhones registered for development.
2. Clone this repo. The Polar BLE SDK is pulled by Swift Package Manager from `https://github.com/polarofficial/polar-ble-sdk.git`, `.upToNextMajor(from: "8.3.0")` (a checkout of the same tag lives in the project vault at `Sources/polar-ble-sdk/` for offline reading).
3. Read `CLAUDE.md`, then `docs/capture-app-build-scaffold.md` §5 (build order) and §7 (acceptance tests) before opening Xcode.
4. Python side: `python3 -m venv .venv && source .venv/bin/activate` — dependencies are declared per folder once code lands.

## Hard rules

- **Never Bluetooth audio; OS effects off and verified** — `AVAudioSession` `.record` + `.measurement`, voice processing off, route and sample rate read back and logged; any failed effect-disable blocks recording.
- **Raw audio, ECG, IMU never in git**, never in a cloud table, never uploaded. `.gitignore` is a backstop, not the rule.
- **No participant identity anywhere** — codes `P01`, `P02`, …; the key lives outside every repo.
- **Stop Polar ECG/ACC streams before disconnecting** (H10 stays on otherwise) and set the H10 clock at session start (it resets when it shuts down).
- No dissemination beyond the class without Prof. Dong's written approval.

## Links

Project vault (private): `gaku-se498` — the design note, decisions, risk register, literature review. Course: Canvas SE 498.

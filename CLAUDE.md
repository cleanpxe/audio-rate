# CLAUDE.md — audio-rate build repo

Read this first in any Claude Code session in this repo.

## What this is

The **code and reproducibility repo** for team Audio Rate's SE498 project (UIUC, Fall 2026, Prof. Yiwen Dong): breath audio → frozen log-mel → CNN + GRU → continuous %HRR, validated against a Polar H10. The planning vault (`~/docus/obsidian/gaku-se498`, private) holds the design, decisions and literature; this repo holds what runs. `README.md` has the layout and the hard rules; `docs/` has the four briefs that specify the work. **The final report must be reproducible from here** — that is graded.

## How AI is used here

Prof. Dong teaches AI-assisted development as method (Lecture 6: Plan mode → frontend → backend, "ask me clarification questions"). So in this repo Claude **may write code with the team**, on these terms: the team reads, understands and owns every line; every change is reviewed by a human before commit; nothing is pasted in that the team could not explain in the final talk. Start in **Plan mode**, ask before building, and keep changes small and reviewable.

## Non-negotiables (from the vault's `CLAUDE.md`, restated for code)

1. **Raw audio, ECG and IMU recordings never enter this repo**, any branch, any commit, any cloud store. Session bundles live on the encrypted drive only (`docs/data-management-plan.md`). If a recording is ever staged, stop and say so.
2. **No participant identity** in code, comments, filenames, tests, fixtures or commit messages. Codes only.
3. **Capture correctness beats features.** Unprocessed path (`.measurement`, voice processing off, wired or built-in mic, never Bluetooth); read back sample rate, route and effect flags and **hard-stop** on mismatch; 16 kHz WAV to disk, never features-only.
4. **The front end is frozen once ported** (`features/`): 25 ms Hann, 10 ms hop, 64 log-mels, fixed floor clamp, calibration-tone normalisation — never per-clip mean-variance. Byte-identical between app and training; the parity test is mandatory.
5. **One clock.** Mach host time anchors audio, IMU, strap arrivals and events; Polar sample timestamps (ns since 2000-01-01) are logged alongside, never trusted alone; the clap on the electrodes aligns them offline.
6. **Polar H10 handling:** `setLocalTime` at session start; stop ECG/ACC streams before disconnect; only one BLE client gets RR at a time (trainer app on a different device).
7. **Report what is.** Failing tests, missing readbacks and skipped steps are stated plainly, never smoothed over.

## Where things are specified

- `docs/capture-app-build-scaffold.md` — module map, clock strategy, verified Polar SDK surface (8.3.0), build order, the ten acceptance tests that define "Phase 0 done".
- `docs/data-management-plan.md` — session-bundle layout (`Pxx_YYYY-MM-DD/`: `audio_A.wav`, `imu_A.csv`, `h10_rr.csv`, `h10_ecg.csv`, `h10_acc.csv`, `events.csv`, `ratings.csv`, `metadata.json`, `qa.json`, `derived/`), what may be released, retention.
- `docs/lab-session-runbook.md` — the session the app has to serve, step by step.
- `docs/pipeline-input-to-verdict.md` — every stage from mic to zone with its silent failure.
- Polar BLE SDK: use SPM `polarofficial/polar-ble-sdk` at `.upToNextMajor(from: "8.3.0")`. The SDK repo ships `AGENTS.md` and `sources/iOS/AGENTS.md` for coding agents — **read them from the checkout before writing SDK code; do not assume API details from memory.** 8.x is async/await (`AsyncThrowingStream`), not RxSwift.

## Conventions

- Swift: SwiftUI unless the team decides otherwise (open decision in the scaffold §6); one module per folder under `capture/`, named as in the scaffold's module map (AudioCapture, Clock, MotionLogger, StrapClient, EventMarks, Qualification, SessionBundle).
- Python: `features/`, `labels/`, `model/`, `eval/` are packages with a `README.md` each; pin dependencies; scripts take a session-bundle path, never a participant name.
- Tests: every acceptance test in the scaffold §7 gets an automated check where one is possible (parity, bundle schema, clock offsets); the acoustic ones (cal-tone linearity, pink-noise gating) run through `eval/` on a recorded bundle.
- Commits: plain imperative, one concern each. Never commit generated `DerivedData`, `xcuserdata`, models or data.
- Before claiming a phase is done, run the acceptance list and paste the results into the PR or commit body.

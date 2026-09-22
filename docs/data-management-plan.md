# Data Management Plan

**SE498 Team 3 "Audio Rate" · what is collected, where it lives, who sees it, what leaves · state as of 2026-09-16**

## For future Claude
The operational half of Audio Data Ethics and Handling: session-bundle layout, naming, storage, access, the speech-review step, what the public repo ships, and the retention and deletion promises made in Consent Form - Audio Rate. Everything here follows the hard rules in `CLAUDE.md` (raw audio never in git or the vault; participant identities never in notes). ⬜ = team decision still open. Companion: Lab Session Runbook (produces the bundle), Pipeline Input to Verdict (consumes it).

---

## 1. What a session produces

One **session bundle** per participant, one folder, named by code and date: `P07_2026-10-02/`

| File | Content | Source |
|---|---|---|
| `audio_A.wav`, `audio_B.wav`, (`audio_C.wav`) | 16 kHz mono WAV per phone, whole session | capture app |
| `imu_A.csv`, … | accelerometer + gyroscope, audio-clock timestamps | capture app (Decision - IMU logged by the capture app) |
| `h10_rr.csv` | RR intervals with BLE arrival and strap timestamps | app, Heart Rate Service |
| `h10_ecg.csv`, `h10_acc.csv` | raw ECG (~130 Hz) and chest accelerometer | app, Polar SDK (Decision - Breathing-rate reference H10 first belt conditional) |
| `events.csv` | every event button press with clock time | app |
| `ratings.csv` | Borg overall / legs / breathing per stage and per 5 min of the bout | typed from the sheet |
| `metadata.json` | session-sheet fields (Lab Session Runbook §6): phones, gain, distances, wattages, room temperature, water, caffeine, sleep, Tanaka HRmax, resting HR, readbacks | typed + app export |
| `qa.json` | QA outcomes: cal-tone level, floor, clap offsets, RR rejection rate, lag, breathing-rate check, speech strip list | QA script + reviewer |
| `derived/` | log-mel features, labels, windows (regenerable) | pipeline |

Nothing in the bundle carries a name. The bundle is **the** unit of QA and of deletion.

## 2. Where it lives

| Copy | Location | Who | Encryption |
|---|---|---|---|
| Capture | the phones, until copied the same day, then deleted from the phones | experimenter | device encryption |
| **Master** | encrypted external drive ⬜ (or university-approved encrypted storage ⬜) held by Josh | Josh + ⬜ | required |
| Working | the modelling laptop, `derived/` and bundles for the current experiment only | Josh | full-disk encryption |
| Never | the vault, GitHub, chat uploads, cloud drives without approval, email | — | — |

The **code-to-name key** and the signed consent forms live in a separate physical folder ⬜, never on the same drive as the bundles.

## 3. Speech review (the consent promise)

Breath audio near the mouth records anything said. Per session, after QA: run voice-activity detection on every audio file → a human listens to each flagged segment → segments with speech are listed in `qa.json` with start/end. Those segments are **excluded from every feature table and released artefact**; the raw WAV is not edited (it stays encrypted). The rating exchanges are speech too — they are stripped the same way, which is why ratings are typed from the sheet, not transcribed.

## 4. What leaves the master drive

| Artefact | Leaves to | Rule |
|---|---|---|
| Per-window log-mel features + labels | the repo, as the reproducibility dataset | only after the **re-identification audit** (Risk 14: Paper - Lu 2020 BreathID breath biometrics from speech, Paper - Tran 2023 Breath sound person identification noisy) — if released features identify participants above chance, release coarser features or none |
| Model weights, evaluation scripts, session `metadata.json` (no names), `qa.json` | the repo | yes |
| Aggregate plots, ledger rows | this vault, the report | yes |
| Raw audio, ECG, IMU | nowhere | never; on request to Prof. Dong in person, de-identified, if she asks |
| Anything | outside the class | only with Prof. Dong's **written** approval |

## 5. Retention and deletion (as consented)

- Deletion on request until **2026-11-20** (the feature-freeze date; change here and on the form together). A request deletes the bundle, its `derived/`, and its rows from any feature table not yet frozen; the code stays in the key sheet marked *withdrawn*.
- After the freeze, de-identified features may already be in the analysis and cannot be traced back — the form says so.
- Raw audio, ECG and IMU: deleted, or kept only in encrypted archive, within **twelve months of the end of the course** (by 2027-12-31). Owner ⬜.
- Consent forms and the key: kept by ⬜ for the same period, then shredded.

## 6. Repo layout (reproducibility is graded)

`github.com/cleanpxe/audio-rate` — `capture/` (iOS app) · `features/` (frozen front-end, shared with the app) · `labels/` (RR → %HRR, lag fit) · `model/` · `eval/` (LOPO, LODO, per-phase, zones, baselines) · `data/` (released features + data card only) · `docs/` (this plan, the runbook, the pipeline note as exported PDFs). A **data card** states n, sessions, devices, sample rate, exclusion rules, and what was stripped. `.gitignore` blocks every audio extension.

## Connections

- Audio Data Ethics and Handling · Consent Form - Audio Rate · Participant Consent Script and Form · Lab Session Runbook · Pipeline Input to Verdict · Risk Register rows 3, 14 · Project Guidelines · Proposal §4


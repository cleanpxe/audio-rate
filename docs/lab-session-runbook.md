# Lab Session Runbook

**SE498 Team 3 "Audio Rate" · one participant, one visit, about 65 minutes · state as of 2026-09-16 · every step traces to an accepted decision; ⬜ marks what the team still fills in**

## For future Claude
The experimenter-facing procedure for a collection session, assembled from the protocol decisions of 2026-09-16 (Decision - Protocol one session per participant age-predicted HRmax · Decision - Recovery window recorded from session one · Decision - Labels by fitted lag continuous exertion trajectory · Decision - Breathing-rate reference H10 first belt conditional · Decision - IMU logged by the capture app · Decision - Handset roster three iPhones) and the capture rules in Audio-Derived Exercise Exertion Estimation §1–4. Companion documents: Consent Form - Audio Rate (what the participant signs), Data Management Plan (where the files go), Pipeline Input to Verdict (what happens to the data). PDF beside this note is footer-free so it can be printed for the lab. Update when an ADR changes; re-export with `--no-foot`.

---

## 0. Before the day

| Item | Status | Owner |
|---|---|---|
| Ergometer: Jude's Wahoo KICKR Core, ERG mode, bike mounted, controlling app on a device other than the strap-ingest phone (Decision - Jude's Wahoo KICKR as the cycle ergometer) | ⬜ | Jude |
| Quiet room booked (Risk 5) — settled 09-22: the small apartment room where the KICKR Core lives; confirm it is free for the slot, HVAC and fans off during recording | ⬜ | ⬜ |
| Polar H10 in hand; nRF Connect shows the RR flag; Polar SDK streams ECG + ACC | ⬜ | Josh |
| Three iPhones listed (model, iOS version), each passed the qualification test in the last 30 days | ⬜ | ⬜ |
| Capture app: `.measurement` mode, voice processing off, 16 kHz WAV, IMU logging, H10 ingestion, event buttons, file-injection parity test green | ⬜ | Josh |
| Wired headset boom mic + foam windscreen; calibration-tone source; tape for the fixed geometry | ⬜ | ⬜ |
| Printed consent form ×1, health screen ×1, Borg 6–20 card, differentiated-rating card, session sheet | ⬜ | ⬜ |
| Participant code assigned in the key sheet (kept outside the vault) | ⬜ | Josh |
| Water bottle for the participant; thermometer or the room's reading | ⬜ | ⬜ |

## 1. Arrival and consent (10 min, not recorded)

1. Welcome; read the script in Participant Consent Script and Form aloud; answer questions.
2. Health screen first (§8 of the form). Any "yes" → thank them, stop, no data.
3. Consent initials and signatures. Form goes in the consent folder, never near the data.
4. Record on the session sheet: participant code, date, **room temperature**, water offered (yes/no), caffeine in the last 3 h, hours of sleep, training background if the optional line was initialled (Decision - Protocol one session per participant age-predicted HRmax).
5. Compute Tanaka HRmax = 208 − 0.7 × age; write it on the sheet. Never ask the participant to ride to exhaustion.

## 2. Setup (10 min)

1. **Strap:** wet the electrodes; fit the Polar H10 under the shirt, snug. Start the app's H10 stream; confirm RR intervals and ECG arrive; watch for the dry-electrode garbage to clear.
2. **Microphone:** wired headset boom at the fixed geometry — near the mouth corner, off-axis from the nostrils, windscreen on. Tape the boom position. Plug into iPhone A. Mount iPhone B (and C if used) at the logged distance ⬜ on the handlebar mount.
3. **Gain:** set the input gain to the study value ⬜ and **do not touch it again**. Log the value. A mid-session gain change invalidates the session's level comparisons.
4. **App readback:** confirm `.measurement` mode, voice processing off, input route, sample rate, IMU on. Any failed readback is a hard stop, not a warning.
5. Seated rest begins as soon as the strap is stable — see §3.

## 3. Recorded session (≈ 45 min riding + rest)

Start **one continuous recording per phone** at the beginning of seated rest and stop it after the final recovery. Press the app's event button at every transition; write the clock time on the sheet as a backup.

| Step | Duration | What the experimenter does | Event |
|---|---|---|---|
| **Seated rest** | 5 min | Participant sits still, no talking. Resting HR = last 3 min. | `rest_start` |
| **Pre-roll** | 1 min | Calibration tone from the reference source at the taped distance (10 s) → 10 s room tone (silence) → **sync clap**: one hand clap while the other hand touches the strap electrodes, visible in audio and RR. | `cal_tone`, `room_tone`, `clap` |
| **Practice ride** | 3 min | Light load. Explain the Borg 6–20 scale and the three differentiated ratings (overall / legs / breathing). Take one practice rating. | `practice_start` |
| **Warm-up** | 3 min | Fixed low wattage ⬜. Standardised starting state; not a lag fix. | `warmup_start` |
| **Stages** | 3–4 × 3–4 min | Wattage steps ⬜ (rising to "hard", RPE ≈ 15–16). In the **last 30 s of every stage**: Borg overall, legs, breathing. Participant points; experimenter says the numbers aloud once (they are on the recording; fine) and writes them. | `stage_N_start`, `rpe_N` |
| **Recovery 1** | 3–5 min | Unloaded pedalling or seated. Keep recording — this is D1 data (Decision - Recovery window recorded from session one). | `recovery1_start` |
| **Drift bout** | 15–20 min | Constant moderate load ⬜ W (≈ 60 % of the participant's top stage). Ratings **every 5 min** and at the end. Extend from 15 to 20 min if the participant and the clock allow. Water available. | `bout_start`, `rpe_bout_k` |
| **Recovery 2** | 3–5 min | As Recovery 1. | `recovery2_start` |
| **Stop** | — | Stop recordings on every phone, then the H10 stream. | `session_end` |

**Stop rules, spoken before the practice ride:** the participant says "stop" · chest discomfort · dizziness · unusual breathlessness · pain · the experimenter's judgement. End immediately, keep them seated, let them cool down, log `aborted` and the reason. Aborted sessions are kept but flagged; nothing is "rescued".

**Silence rule:** no conversation during recorded steps except the rating exchange. If the participant speaks, note the time; the speech review in Data Management Plan strips it.

## 4. Teardown (5 min)

1. Strap off; participant offered water; thank them; remind them of the deletion deadline on the form.
2. Copy the session bundle from every phone to the encrypted drive **before leaving the room** (Data Management Plan §2). Verify file sizes are non-zero and durations match.
3. Fill the QA sheet (§5). Wipe the boom windscreen or swap it.

## 5. Same-day QA (30–60 min, experimenter or Josh)

A session enters modelling only when every line passes. Reject, do not rescue.

- [ ] Calibration tone level within ± 1 dB of the study reference on every phone; room-tone floor did not gate to silence; no spectral hole 1–4 kHz (design note §2).
- [ ] Sync clap located in every audio file and in the RR/ECG stream; offsets written to the bundle metadata.
- [ ] Event log complete; stage times agree with the sheet within 5 s.
- [ ] RR rejection rate below ⬜ % per stage; no cadence-locking (HR tracking pedal cadence).
- [ ] Lag fit runs and lands inside 0–90 s with a single clear peak (Decision - Labels by fitted lag continuous exertion trajectory; Risk 10).
- [ ] H10-derived breathing rate (ECG and ACC) computed for the bout and the rest period; **pilot one only:** ± 3 br/min vs a manual count at rest and at the top stage decides the belt (Decision - Breathing-rate reference H10 first belt conditional).
- [ ] Speech segments flagged by VAD and reviewed; strip list written.
- [ ] IMU file present for every phone, timestamps on the audio clock.

## 6. Session sheet fields (paper, then typed into the bundle's `metadata.json`)

participant code · date · experimenter · room temperature · water offered / intake · caffeine (h since) · sleep (h) · age (for Tanaka) · training background (optional) · Tanaka HRmax · resting HR · phones used (model, iOS, mount, gain) · boom distance · wattage per stage · bout wattage · ratings table · events with clock times · stop rule used (if any) · QA outcome.

## Connections

- Consent Form - Audio Rate · Participant Consent Script and Form · Data Management Plan · Pipeline Input to Verdict · Equipment and Purchase List · Audio-Derived Exercise Exertion Estimation §1–4 · Ground Truth and Validation · Risk Register rows 5, 8, 10, 11 · Tasks


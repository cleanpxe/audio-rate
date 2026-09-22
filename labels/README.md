# labels/ — RR intervals → %HRR, and the lag fit

From a session bundle: `h10_rr.csv` → instantaneous HR → rejection (> ~20 % off the local median; cadence-lock check) → discard the dry-electrode window → resample to 1 Hz → ~10 s smoothing → **%HRR** by Karvonen with the session resting HR and **Tanaka HRmax = 208 − 0.7 × age**. Then the per-participant **lag** between the audio energy envelope (300–3000 Hz RMS, 1 s frames) and the HR series by cross-correlation, constrained to 0–90 s, applied so each 10 s window gets the %HRR it belongs to. Report MAE separately for ramp / steady / recovery downstream.

Never train on raw BPM.

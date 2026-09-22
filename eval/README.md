# eval/ — QA, splits, metrics

1. **Session QA script** (runs on a bundle the same day): cal-tone linearity across three levels, room-tone floor and spectral holes, clap offsets across audio / IMU / ECG, RR rejection rate, lag estimate, breathing-rate check vs manual count, speech-strip list → writes `qa.json`. Reject bad sessions; don't rescue.
2. **Splits:** leave-one-participant-out (main result) and leave-one-device-out across the three iPhones (headline); a locked participant test set until the end.
3. **Metrics:** %HRR MAE / RMSE with Bland–Altman; per-phase MAE (ramp / steady / recovery); zone accuracy strict and ± 1 zone; per-participant MAE beside the pooled number; incremental validity of audio over HR against Borg (mixed model, HR first, within-subject).

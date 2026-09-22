# features/ — the frozen front end

Log-mel front end shared byte-for-byte by the app (Swift/Accelerate or the Core ML preprocessing path) and the training pipeline (Python): **16 kHz · 25 ms Hann · 10 ms hop · 64 mel bands · log(max(S, ε)) with fixed ε · 10 s window / 5 s hop · normalisation = offset so the calibration tone lands at a fixed reference level.** Never per-clip mean-variance.

Deliverables: the reference Python implementation, the Swift port, a fixed test WAV (synthetic, not a participant recording) and the **parity test** comparing the two numerically. Front-end mismatch across phones is a silent domain shift — this folder is where that is prevented.

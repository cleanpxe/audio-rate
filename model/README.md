# model/ — CNN + GRU regression, baselines, export

Small CNN over each 10 s log-mel window → embeddings → GRU (or attention) over 30–60 s of context → %HRR regression head, Huber loss; auxiliary scalar input resting HR; auxiliary breathing-rate head. Baselines that must be reported beside it: hand-feature model (RMS, crest factor, ACF breathing rate, Welch band power → tree/SVM), **IMU-only**, **power-only** (trainer wattage). Export: Core ML, int8 with a calibration set, quantised ≈ float on test vectors.

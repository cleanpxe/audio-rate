# audio-rate

**Acoustic exercise-exertion estimation from breath audio** — UIUC SE 498 (AI-Driven Intelligent Systems), Fall 2026, Prof. Yiwen Dong. Team 3 **Audio Rate**: Jude Welsch · Joshua Jimenez · Ray Chang.

A phone or wired-headset microphone records breathing during cycle-ergometer exercise; a frozen log-mel front end and a small CNN + GRU regress continuous exertion as %HRR per 10-second window, validated against a Polar H10 chest strap on held-out participants and held-out handsets.

This is the **release repo**: finished, reviewed code lands here at each project milestone, and the final report (December 2026) is reproducible from it. It contains no raw audio, ECG or motion recordings and no participant-identifying data. Released features and the data card appear only after a re-identification audit, per the course's ethical-research rules.

Layout on release: `capture/` (iOS app) · `features/` (frozen front end) · `labels/` · `model/` · `eval/` · `data/` · `docs/`.

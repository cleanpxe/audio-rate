# data/ — released features and the data card only

What may live here, and only after the re-identification audit: per-window log-mel features + %HRR labels, session `metadata.json` (no names), `qa.json`. **Never** raw audio, ECG or IMU — those stay on the encrypted drive and are deleted per the consent form's retention rule.

## Data card (fill before release)

- Participants (n), sessions, age range, training status (aggregate only)
- Devices: iPhone models and iOS versions; microphone route; Polar H10 firmware
- Sample rate and front-end parameters; window / hop
- Exclusion rules applied and how many windows / sessions they removed
- What was stripped (speech review outcome) and the re-identification audit result
- Licence and contact (team email), and the written-approval status for any use beyond the class

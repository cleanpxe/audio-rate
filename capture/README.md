# capture/ — iOS capture app (AudioRate Capture)

Bespoke iOS app for data collection. Scope, module map, clock strategy, Polar SDK surface, build order and acceptance tests: **`../docs/capture-app-build-scaffold.md`**.

Nothing here yet. First build target (Phase 0, initial testing): AudioCapture + Clock with readbacks → MotionLogger → StrapClient (HR/RR, then ECG, then ACC) → EventMarks + SessionBundle → Qualification marks. No accounts, no upload, no Supabase.

Polar BLE SDK via Swift Package Manager: `https://github.com/polarofficial/polar-ble-sdk.git`, `.upToNextMajor(from: "8.3.0")`. Plist: `NSBluetoothAlwaysUsageDescription`, `NSMicrophoneUsageDescription`, `NSMotionUsageDescription`. Background Modes: *Uses Bluetooth LE accessories*, *Audio*.

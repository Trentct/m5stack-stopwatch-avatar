# Contributor and agent guidance

Before changing the firmware:

1. Read `README.md`, `docs/HARDWARE_BASELINE.md`, and `docs/ENGINEERING_NOTES.md`.
2. Preserve the product visual boundary: pure-black background with white or light-gray procedural eyes; no image-frame animation unless a proposal explicitly changes the renderer.
3. Keep official hardware capability, source integration, successful compilation, and real-device verification as separate evidence levels.
4. Run `pio run` as the minimum acceptance check.
5. Do not report touch, IMU, vibration, buttons, display quality, or performance as device-verified until the firmware has been uploaded and observed on a physical StopWatch.
6. Do not commit `.pio`, firmware binaries, serial-device identifiers, credentials, or private device logs.

Changes to rendering or interaction should include before/after behavior, measured frame rate when relevant, and the physical-device test still required.

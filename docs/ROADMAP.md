# Roadmap

This roadmap separates implemented firmware from future hardware experiments.

## Current foundation

- 12 procedural expressions and reusable animation timelines;
- persistent base states with temporary reactions that return to the active base;
- touch following, click, double-click, long-press and continuous swipe;
- accelerometer/gyroscope tilt following and repeated horizontal-shake detection;
- A/B expression browsing, vibration feedback and hardware diagnostics;
- serial semantic commands for external controllers;
- 60 fps target renderer with dynamic dirty rectangles.

## Near-term work

- add a short real-device demo video and interaction test matrix;
- expose touch, tilt and shake thresholds as a small calibration profile;
- add regression checks for expression catalogue and playback transitions;
- package signed firmware binaries and checksums in GitHub Releases;
- measure power consumption and define an always-on versus sleep strategy.

## Exploratory work

- microphone capture and offline command-word recognition;
- audio reactions through the onboard codec and speaker;
- RTC-aware behavior;
- phone, Wi-Fi or external-controller integrations;
- a compact authoring/export path for new procedural expressions.

These items are not shipped capabilities until they are implemented, compiled, uploaded and observed on a physical device.

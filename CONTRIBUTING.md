# Contributing

Thanks for helping improve M5Stack StopWatch Avatar.

## Before opening a change

1. Describe the user-visible behavior you want to change.
2. Keep rendering and hardware input changes small enough to test independently.
3. Run the firmware build:

```sh
pio run
```

4. If the change affects display, touch, IMU, buttons or vibration, upload it to a physical StopWatch and report what was actually observed.

## Pull request checklist

- [ ] `pio run` succeeds.
- [ ] No `.pio` directory, firmware binary, serial-device identifier or credential is committed.
- [ ] New expressions preserve the default pure-black monochrome visual boundary.
- [ ] Interaction changes document thresholds and expected recovery behavior.
- [ ] Hardware claims distinguish compilation from physical-device verification.
- [ ] Public behavior or setup changes update the README or relevant docs.

## Bug reports

Please include:

- hardware model and firmware commit;
- exact interaction sequence;
- expected and actual behavior;
- relevant serial output with device identifiers removed;
- a short device video when the issue is visual or gesture-related.

## Style

- Follow the existing C++ formatting and naming.
- Keep frame-loop work lightweight and avoid display readback on the animation path.
- Prefer reusable expression/keyframe data over one-off drawing branches.
- Add comments for coordinate transforms, thresholds and performance-sensitive code.

By contributing, you agree that your contribution is licensed under AGPL-3.0-or-later.

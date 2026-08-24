# M5Stack StopWatch Avatar

[![Build firmware](https://github.com/Trentct/m5stack-stopwatch-avatar/actions/workflows/build.yml/badge.svg)](https://github.com/Trentct/m5stack-stopwatch-avatar/actions/workflows/build.yml)
[![License: AGPL v3](https://img.shields.io/badge/License-AGPL_v3-blue.svg)](LICENSE)

An expressive, monochrome procedural avatar firmware for the M5Stack StopWatch.

这是一个运行在 M5Stack StopWatch 圆形 AMOLED 屏幕上的程序化表情设备。所有眼睛、眼皮、眉线、关键帧和过渡均由 C++ 实时绘制，不依赖图片序列帧。产品画面保持纯黑背景，并针对 466 × 466 圆屏优化了尺寸、局部刷新和交互范围。

> Community project. Not affiliated with or endorsed by M5Stack.

## Highlights

- 12 种程序化表情：`idle`、`listening`、`thinking`、`happy`、`excited`、`curious`、`confused`、`angry`、`surprised`、`sad`、`sleepy`、`dizzy`；
- 60 fps 目标渲染，使用动态脏矩形降低 AMOLED 刷新开销；
- 点击、双击、长按、连续触摸跟随和上下/左右滑动；
- 加速度计与陀螺仪共同驱动倾斜跟随，眼睛先移动、头部稍后跟随；
- 连续 4 次强力左右往复摇动触发旋涡眩晕；
- A/B 实体键浏览表情，震动马达提供反馈；
- A+B 长按进入硬件诊断；
- 串口语义命令可作为未来语音识别或外部控制模块的统一入口。

## Interaction map

| Input | Result |
| --- | --- |
| 单击 | `happy` |
| 双击 | `surprised` |
| 按住并移动 | 眼睛和头部连续跟随触点 |
| 长按 | `angry` |
| 左右滑动 | 跟手预览并切换相邻表情 |
| 向上 / 向下滑动 | `surprised` / `sleepy` |
| 缓慢倾斜 | 视线按倾斜方向连续移动 |
| 4 次强力左右往复摇动 | 循环 `dizzy`，停稳后恢复 |
| A / B | 上一个 / 下一个表情 |
| 长按 A+B | 进入 / 退出硬件诊断 |

`idle`、`listening` 和 `thinking` 是可持续停留的基础状态。其他反应播放完成后会返回触发前的基础状态，而不是固定回到待机。

## Hardware

- [M5Stack StopWatch Dev Kit (C152)](https://docs.m5stack.com/en/core/StopWatch)
- ESP32-S3R8, 16 MB Flash, 8 MB PSRAM
- 1.75-inch 466 × 466 circular AMOLED touch display
- BMI270 six-axis IMU
- CST820B touch controller
- Two programmable buttons and an internal vibration motor

See [Hardware baseline](docs/HARDWARE_BASELINE.md) for interfaces, addresses and the current verification boundary.

## Build

Requirements:

- [PlatformIO Core](https://platformio.org/) 6.1.18
- USB-C data cable
- M5Stack StopWatch

The library commits used by the verified build are pinned in [`platformio.ini`](platformio.ini).

```sh
pio run
```

## Upload and monitor

Connect the StopWatch over USB-C. If automatic upload does not start, hold reset for about two seconds and release it when the green LED turns on.

```sh
pio run --target upload
pio device monitor --baud 115200
```

The monitor accepts expression names such as `happy`, `thinking` or `dizzy`. Playback testing also supports:

```text
once <expression>
loop <expression>
pingpong <expression>
```

## Repository map

| Path | Purpose |
| --- | --- |
| `src/avatar_engine.*` | Expression catalogue, timelines, easing, drawing and interaction physics |
| `src/main.cpp` | Device setup, touch/IMU/buttons, vibration, diagnostics and serial commands |
| `docs/HARDWARE_BASELINE.md` | Hardware capabilities and verification boundary |
| `docs/ENGINEERING_NOTES.md` | Rendering experiments, measurements and implementation decisions |
| `docs/ROADMAP.md` | Planned work and intentionally unsupported features |

## Known limitations

- The microphone and offline speech recognition are not connected yet. Serial commands only simulate semantic voice events.
- Audio playback, RTC, deep sleep, wake-up strategy and external expansion ports are not integrated.
- Battery life has not been optimized for long-term always-on use.
- Subjective motion and gesture tuning may vary with how the device is held.

## Inspiration and provenance

This project was inspired by the expression/animation/playback layering of [Bible Strong Avatar Lab](https://github.com/smontlouis/bible-strong-avatar-lab). It is an independent C++ implementation rebuilt for ESP32 hardware and does not bundle the upstream web application, TypeScript source, exported avatar data or visual assets.

The concise relationship is: **Inspired by the architecture, rebuilt for completely different hardware.**

Hardware initialization, pin mapping and IMU screen-axis handling reference M5Stack's official [StopWatch User Demo](https://github.com/m5stack/M5StopWatch-UserDemo). See [Third-party notices](THIRD_PARTY_NOTICES.md) for details.

## Contributing

Issues and pull requests are welcome. Please read [CONTRIBUTING.md](CONTRIBUTING.md) and run `pio run` before submitting a change. Hardware-dependent claims should include real-device evidence when possible.

## License

This project is licensed under the [GNU Affero General Public License v3.0 or later](LICENSE).

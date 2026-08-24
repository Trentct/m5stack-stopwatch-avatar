# KK — M5Stack StopWatch 表情角色

[English](README.md) | [简体中文](README.zh-CN.md)

[![构建固件](https://github.com/Trentct/m5stack-stopwatch-avatar/actions/workflows/build.yml/badge.svg)](https://github.com/Trentct/m5stack-stopwatch-avatar/actions/workflows/build.yml)
[![许可证：AGPL v3](https://img.shields.io/badge/License-AGPL_v3-blue.svg)](LICENSE)

认识一下 **KK**：一个住在 M5Stack StopWatch 里的小表情角色。

KK 运行在 M5Stack StopWatch 的圆形 AMOLED 屏幕上。它的眼睛、眼皮、眉线、关键帧和过渡均由 C++ 实时绘制，不使用图片序列帧。产品画面保持纯黑背景，并针对 466 × 466 圆屏、局部刷新和直接交互进行了优化。

> 这是一个社区项目，与 M5Stack 官方没有隶属或背书关系。

## 主要特点

- 12 种程序化表情：`idle`、`listening`、`thinking`、`happy`、`excited`、`curious`、`confused`、`angry`、`surprised`、`sad`、`sleepy`、`dizzy`；
- 以 60 fps 为目标渲染，使用动态脏矩形减少 AMOLED 传输开销；
- 支持单击、双击、长按、连续触摸跟随和上下/左右滑动；
- 加速度计与陀螺仪共同驱动倾斜跟随，眼睛先移动、头部稍后跟随；
- 连续 4 次强力左右往复摇动会触发循环的旋涡眩晕；
- A/B 实体键浏览表情，并由震动马达提供反馈；
- 长按 A+B 进入硬件诊断；
- 串口语义命令为未来语音识别或外部控制提供稳定入口。

## 交互方式

| 操作 | 结果 |
| --- | --- |
| 单击 | `happy` |
| 双击 | `surprised` |
| 按住并移动 | 眼睛和头部持续跟随触点 |
| 长按 | `angry` |
| 左右滑动 | 跟手预览并切换相邻表情 |
| 向上 / 向下滑动 | `surprised` / `sleepy` |
| 缓慢倾斜设备 | 视线持续跟随倾斜方向 |
| 4 次强力左右往复摇动 | 循环 `dizzy`，设备停稳后恢复 |
| A / B | 上一个 / 下一个表情 |
| 长按 A+B | 进入 / 退出硬件诊断 |

`idle`、`listening` 和 `thinking` 是可持续停留的基础状态。其他反应播放完成后会返回触发前的基础状态，而不是固定回到待机。

## 硬件

- [M5Stack StopWatch Dev Kit（C152）](https://docs.m5stack.com/en/core/StopWatch)
- ESP32-S3R8、16 MB Flash、8 MB PSRAM
- 1.75 英寸 466 × 466 圆形 AMOLED 触摸屏
- BMI270 六轴惯性传感器
- CST820B 触摸控制器
- 两个可编程按键和内置震动马达

接口、地址和当前验证边界请参阅[硬件基础资料](docs/HARDWARE_BASELINE.md)。

## 编译

需要准备：

- [PlatformIO Core](https://platformio.org/) 6.1.18
- USB-C 数据线
- M5Stack StopWatch

已经验证的依赖提交均锁定在 [`platformio.ini`](platformio.ini) 中。

```sh
pio run
```

## 烧录与串口监视

通过 USB-C 连接 StopWatch。如果没有自动开始烧录，可以按住复位键约两秒，在绿色 LED 亮起时松开。

```sh
pio run --target upload
pio device monitor --baud 115200
```

串口监视器接受 `happy`、`thinking`、`dizzy` 等表情名称，也支持以下播放测试命令：

```text
once <expression>
loop <expression>
pingpong <expression>
```

## 仓库结构

| 路径 | 用途 |
| --- | --- |
| `src/avatar_engine.*` | 表情目录、时间轴、缓动、绘制和交互物理 |
| `src/main.cpp` | 设备初始化、触摸、IMU、按键、震动、诊断和串口命令 |
| `docs/HARDWARE_BASELINE.md` | 硬件能力和验证边界 |
| `docs/ENGINEERING_NOTES.md` | 渲染实验、测量数据和实现决策 |
| `docs/ROADMAP.md` | 计划工作和暂不支持的能力 |

## 当前限制

- 麦克风和离线语音识别尚未接入，串口命令只是模拟语义语音事件。
- 音频播放、RTC、深度睡眠、唤醒策略和外部扩展口尚未集成。
- 尚未针对长期常亮使用优化续航。
- 动作与手势的主观体验可能会随设备握持方式而变化。

## 灵感与来源

本项目的表情、动画和播放分层受到 [Bible Strong Avatar Lab](https://github.com/smontlouis/bible-strong-avatar-lab) 启发。它是针对 ESP32 硬件重新实现的独立 C++ 项目，不包含上游网页应用、TypeScript 源码、导出的角色数据或视觉素材。

两者的关系可以概括为：**受到架构启发，为完全不同的硬件重新实现。**

硬件初始化、引脚映射和 IMU 屏幕坐标处理参考了 M5Stack 官方的 [StopWatch User Demo](https://github.com/m5stack/M5StopWatch-UserDemo)。详情请参阅[第三方声明](THIRD_PARTY_NOTICES.md)。

## 参与贡献

欢迎提交 Issue 和 Pull Request。修改前请阅读 [CONTRIBUTING.md](CONTRIBUTING.md)，并运行 `pio run`。涉及硬件的结论应尽可能附上真机验证证据。

## 许可证

本项目使用 [GNU Affero General Public License v3.0 or later](LICENSE) 开源。

# M5Stack StopWatch 硬件基础资料

更新日期：2026-08-24

## 设备身份

- 产品：M5Stack StopWatch Dev Kit
- SKU：C152
- 主控：ESP32-S3R8，双核 Xtensa LX7，最高 240MHz
- 官方资料：
  - [M5Stack StopWatch 产品文档](https://docs.m5stack.com/en/core/StopWatch)
  - [M5Stack StopWatch 官方商店页](https://shop.m5stack.com/products/m5stack-stopwatch-dev-kit-esp32-s3)

## 官方硬件规格

| 模块 | 官方规格 |
| --- | --- |
| Flash | 16MB |
| PSRAM | 8MB |
| 无线 | 2.4GHz Wi-Fi |
| 屏幕 | 1.75 英寸圆形 AMOLED 触摸屏，466×466，CO5300，QSPI |
| 触摸 | CST820B |
| 按键 | 2 个可编程按键，另有 1 个电源键 |
| 震动 | 内置震动马达 |
| IMU | BMI270 六轴惯性传感器 |
| 音频 | ES8311 Codec、MEMS 麦克风、AW8737A 功放、1W/8Ω 扬声器 |
| RTC | RX8130CE |
| 电源管理 | M5PM1 多级电源管理 |
| 电池 | 450mAh |
| 输入 | USB Type-C，DC 5V |
| 扩展 | HY2.0-4P 和背部 2.54mm 扩展总线 |
| 尺寸/重量 | 52 × 52 × 15.5mm，约 39g |

## 开发配置

本工程采用 M5Stack 官方文档给出的 PlatformIO 路线：

- Platform：`espressif32 @ 6.12.0`
- Board：`esp32s3box`
- Framework：Arduino
- Flash 分区：`default_16MB.csv`
- PSRAM 内存模式：`qio_opi`
- 串口速率：115200
- 核心库：M5Unified、M5GFX、M5PM1、M5IOE1

这里的 `esp32s3box` 是官方示例采用的 PlatformIO board 配置名，不代表设备型号是 ESP32-S3-BOX。

## 常用接口与地址

| 功能 | 接口/地址 |
| --- | --- |
| 系统 I²C | SDA = G47，SCL = G48 |
| BMI270 | I²C `0x68` |
| RX8130CE | I²C `0x32` |
| M5IOE1 | I²C `0x4F` |
| ES8311 | I²C `0x18` |
| A 键（黄色） | G2 |
| B 键（蓝色） | G1 |
| HY2.0-4P Port A | G10 / G11 / 5V / GND |

显示屏复位、触摸复位、音频电源、扬声器功放、震动和部分扩展口切换由 M5IOE1 管理。不要把这些当成普通 ESP32 GPIO 直接驱动。

## 当前软件接入状态

| 能力 | 当前固件接入 | 验证边界 |
| --- | --- | --- |
| AMOLED 显示 | 12 种程序化表情、局部刷新和诊断界面 | 已编译、烧录并持续显示；稳定动画窗口约 60 fps |
| BMI270 | 屏幕坐标倾斜、陀螺仪前馈、横向摇动计数和诊断数据 | 已在真机观察倾斜与 `DIZZY` 触发/恢复；不同握持方式仍需调节 |
| A 键 | 上一个表情；诊断模式震动测试 | 已接入并用于原型浏览；发布前仍建议回归测试 |
| B 键 | 下一个表情；诊断模式重绘 | 已接入并用于原型浏览；发布前仍建议回归测试 |
| 震动马达 | 表情反馈和诊断测试 | PWM 开关链路已运行；主观强度依设备与握持方式而异 |
| 触摸 | 点击、双击、长按、连续跟随和上下/左右滑动 | 已在真机逐轮调节；圆屏边缘和不同手指速度仍值得回归 |
| 麦克风/扬声器 | 未接入 | 未验证，不属于当前发布功能 |
| RTC | 未接入 | 未验证，不属于当前发布功能 |
| 低功耗/唤醒 | 未接入 | 未验证，不属于当前发布功能 |
| 外部扩展口 | 未接入 | 未验证，不属于当前发布功能 |

当前表情固件已完成本地编译和多轮真机烧录。串口轮测确认 12 条语义命令均可进入对应表情，并验证持续基础状态、短暂反应返回、触摸、滑动与 IMU 交互可以和约 60 fps 渲染共同运行。视觉观感、手势阈值、震动强度和续航仍属于具体设备上的主观验收项。

## 震动马达说明

官方 PinMap 表明震动马达由 M5IOE1 的 `PYG9 / PYB_MT_PWM` 控制。本工程沿用已接入的 M5IOE1 库映射：

```cpp
ioe.begin(&M5.In_I2C, 0x4F, M5IOE1_I2C_FREQ_100K);
ioe.setPwmFrequency(200);
ioe.analogWrite(M5IOE1_PWM_CH1, strength);
```

当前版本的 M5IOE1 库将 `M5IOE1_PWM_CH1` 映射到 IO9，与官方震动控制脚一致。

## 需要特别注意

- 圆屏边缘不可用空间较多，重要文字和按钮不要贴近四角。
- 屏幕、Wi-Fi、音频和持续高频刷新都会明显影响 450mAh 电池续航。
- 后续如果产品需要长期待机，应单独设计屏幕熄灭、M5PM1 电源域和 IMU/RTC 唤醒方案。
- 后部 `MUX_IO_1/2` 可在 UART 与 USB 功能之间切换，由 M5IOE1 管理，修改前需要核对官方 PinMap。
- 烧录、音频、RTC、深度睡眠和扩展口都必须在真机上分别验收。

# AtomS3R OpenAI 实时聊天例程

本 demo 属于 esp_webrtc_solution 的一个子项目，如对其他功能感兴趣，请查看[此 README ](https://github.com/espressif/esp-webrtc-solution)。

## 概述

这个例程展示了使用 `esp_webrtc` 与 OpenAI 建立实时聊天连接。它还演示了如何通过函数调用语音命令进行设备控制，并且已经适配了 AtomS3R。

## 与 OpenAI Realtime Embedded SDK 的比较

这个例程提供了类似于 [OpenAI Realtime Embedded SDK](https://github.com/openai/openai-realtime-embedded-sdk) 的功能，并有几个增强：

1. **集成解决方案**：解决了从头构建媒体系统组件的问题，提供了一个随时可用的方案。
2. **优化的编解码器**：利用 `esp_audio_codec` 实现高效的 OPUS 编码和解码功能。
3. **增强型交互功能**：具备声学回声消除（AEC）技术，以提升语音交互的品质。
4. **改进的对等连接**：采用了经过优化的 `esp_peer` 版本，以实现更高的性能和更强的可靠性。
   
## 硬件要求
默认设置使用的是 [ESP32-S3-Korvo-2](https://docs.espressif.com/projects/esp-adf/en/latest/design-guide/dev-boards/user-guide-esp32-s3-korvo-2.html) 。同时我们也为 [AtomS3R](https://shop.m5stack.com/products/atoms3r-dev-kit) 作了适配，可以使用 AtomS3R 搭配 [Atomic Echo Base](https://shop.m5stack.com/products/atomic-echo-base-with-microphone-and-speaker) 运行本例程。

## 编译流程

### IDF 版本
您可以选择 IDF master branch 或者 IDF release v5.4 。

### 依赖

此例程仅依赖于 ESP-IDF。所有其他所需的模块都将自动从 [ESP-IDF 组件注册表](https://components.espressif.com/) 中下载。

### 修改默认设置
1. 更改 Wi-Fi SSID 和密码，请修改 [settings.h](main/settings.h) 中的配置。
2. 配置 OpenAI API key
```bash
export OPENAI_API_KEY=XXXXXXX
```
3. 如果需要适配其他板子，请参考 [codec_board README](https://github.com/espressif/esp-webrtc-solution/blob/main/components/codec_board/README.md)， AtomS3R 已经做了适配，只需要在 [settings.h](main/settings.h) 中把 `EST_BOARD_NAME` 宏定义修改成 `"ATOMS3_ECHO_BASE"` 即可。

### 编译
```
idf.py -p YOUR_SERIAL_DEVICE flash monitor
```

### 测试
在板子启动后，它会尝试连接到所配置的 Wi-Fi。一旦 Wi-Fi 连接成功，它就会自动尝试连接到 OpenAI 服务器。用户还可以使用以下控制台命令来控制会话。
1. `start`: 开始聊天
2. `stop`: 停止聊天
3. `i`: 显示系统负载
4. `wifi`: 连接到一个新的 Wi-Fi SSID 和密码

示例代码中添加了一些预定义的功能调用，包括灯光的开启与关闭、灯光颜色、扬声器音量以及门的开启控制。用户可以通过语音指令来触发这些功能调用。

### 可能遇到的问题
1. 在编译时若遇到 `mspi` 初始化等报错，可以在 `menuconfig` 中将 Flash SPI 的模式修改为 `QIO`, Flash SPI speed 修改为 `120MHz`。
2. 在成功编译烧录后，若使用 ESP-IDF 原生的串口助手测试时遇到程序在 psram 初始化处卡死的情况，请尝试使用第三方的串口助手进行调试。

## 技术细节
要连接至 OpenAI，此示例添加了一个自定义的信令实现 `esp_signaling_get_openai_signaling`。它不使用 STUN/TURN 服务器，因此 `on_ice_info` 中的 `stun_url` 将被设置为 `NULL`。SDP 信息通过 `https_post` API 以 HTTP 请求的形式进行交换。其他所有步骤都遵循 `esp_webrtc` 的典型通话流程。
有关标准连接构建流程的更多详细信息，请参阅[连接构建流程](https://github.com/espressif/esp-webrtc-solution/blob/main/components/esp_webrtc/README.md#typical-call-sequence-of-esp_webrtc)。
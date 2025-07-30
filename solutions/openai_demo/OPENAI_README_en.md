# AtomS3R OpenAI Real-time Chat Demo

This demo is a sub-project of esp_webrtc_solution. If you're interested in other features, please check [this README](https://github.com/espressif/esp-webrtc-solution).

## Overview

This demo showcases establishing a real-time chat connection with OpenAI using `esp_webrtc`. It also demonstrates how to control devices via voice commands using function calls, and has been adapted for AtomS3R.

## Comparison with OpenAI Realtime Embedded SDK

This demo provides functionality similar to [OpenAI Realtime Embedded SDK](https://github.com/openai/openai-realtime-embedded-sdk) with several enhancements:

1. **Integrated Solution**: Provides an out-of-the-box solution without requiring building media system components from scratch.
2. **Optimized Codec**: Implements efficient OPUS encoding/decoding using `esp_audio_codec`.
3. **Enhanced Interaction**: Incorporates Acoustic Echo Cancellation (AEC) technology to improve voice interaction quality.
4. **Improved Peer Connection**: Uses an optimized version of `esp_peer` for better performance and reliability.

## Hardware Requirements
The default configuration uses [ESP32-S3-Korvo-2](https://docs.espressif.com/projects/esp-adf/en/latest/design-guide/dev-boards/user-guide-esp32-s3-korvo-2.html). We've also adapted it for [AtomS3R](https://shop.m5stack.com/products/atoms3r-dev-kit), which can run this demo when paired with [Atomic Echo Base](https://shop.m5stack.com/products/atomic-echo-base-with-microphone-and-speaker).

## Build Process

### IDF Version
You can choose either the IDF master branch or IDF release v5.4.

### Dependencies

This demo only depends on ESP-IDF. All other required modules will be automatically downloaded from the [ESP-IDF Component Registry](https://components.espressif.com/).

### Modify Default Settings
1. To change Wi-Fi SSID and password, modify the configuration in [settings.h](main/settings.h).
2. Configure OpenAI API key
```bash
export OPENAI_API_KEY=XXXXXXX
```
3. If you need to adapt to other boards, please refer to the codec_board README. AtomS3R has already been adapted, just modify the EST_BOARD_NAME macro definition to "ATOMS3_ECHO_BASE" in settings.h.

### Compile
```
idf.py -p YOUR_SERIAL_DEVICE flash monitor
```

### Testing  
After the board boots up, it will attempt to connect to the configured Wi-Fi. Once the Wi-Fi connection is successful, it will automatically try to connect to the OpenAI server. Users can also use the following console commands to control the session:  
1. `start`: Start chatting  
2. `stop`: Stop chatting  
3. `i`: Display system load  
4. `wifi`: Connect to a new Wi-Fi SSID and password  

Some predefined function calls have been added to the sample code, including the turning on and off of lights, light color, speaker volume, and door opening control. Users can trigger these function calls through voice commands.

### Potential Issues
1. If you encounter errors like `mspi` initialization during compilation, you can change the Flash SPI mode to `QIO` and Flash SPI speed to `120MHz` in `menuconfig`.
2. After successful compilation and flashing, if the program gets stuck at psram initialization when testing with ESP-IDF's native serial monitor, try using a third-party serial monitor for debugging.

## Technical Details
To connect to OpenAI, this example adds a custom signaling implementation `esp_signaling_get_openai_signaling`. It does not use STUN/TURN servers, so the `stun_url` in `on_ice_info` will be set to `NULL`. The SDP information is exchanged via HTTP requests using the `https_post` API. All other steps follow the typical call flow of `esp_webrtc`.  

For more details about the standard connection establishment procedure, please refer to [Connection Establishment Flow](https://github.com/espressif/esp-webrtc-solution/blob/main/components/esp_webrtc/README.md#typical-call-sequence-of-esp_webrtc).

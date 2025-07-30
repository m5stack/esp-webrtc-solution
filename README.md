# esp_webrtc_solution

## Introduction

In this fork, we have adapted this functionality for the AtomS3R + Atomic Echo Base solution. If you're interested, please refer to [this tutorial](/solutions/openai_demo/OPENAI_README_EN.md).

This repository provides everything needed to build a WebRTC application.  
It includes the `esp_webrtc` core code along with its dependent components, such as:  
- **`esp_capture`**: For capturing media data  
- **`av_render`**: For playing media data  

Additionally, the repository contains three several demo applications that demonstrate how to use `esp_webrtc`.

## Solutions

### 1. OpenAI Realtime Communication Solution
This demo establishes a WebRTC connection to an OpenAI server for real-time communication.  
It showcases how to use a customized signaling mechanism to build specialized WebRTC applications.

### 2. Doorbell Solution
This demo implements a doorbell application that can:  
- Be controlled in real-time by a browser or phone  
- Send real-time video data to a controller while supporting two-way audio communication

### 3. Peer Demo
This demo mainly show how to use `esp_peer` API to buildup a WebRTC application from scratch.

### 4. Video Call Solution
This demo show how to use `esp_webrtc` data channel to build up video call application.

### 5. WHIP Publisher Solution
This demo show how to use `esp_webrtc` to publish streaming data to WHIP server.

### 6. Doorbell Local Demo
This demo sets up a local doorbell application that operates without external signaling servers.  
An ESP32 series board acts as the signaling server, allowing users to connect directly for WebRTC testing.

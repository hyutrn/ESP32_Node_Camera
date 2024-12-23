# ESP32 Camera Node Project

## Overview

This project implements an ESP32-based camera node using the ESP-IDF framework. The camera captures images and transmits them via MQTT to a remote broker for further processing. The project integrates Wi-Fi connectivity, camera configuration, and MQTT communication.

## Features

- **Camera Initialization**: Configures and initializes the ESP32-CAM module (AI-Thinker).

- **Image Capture**: Captures images in RGB565 format with a frame size of QVGA.

- **MQTT Communication**: Sends image data along with metadata (width, height, etc.) as a JSON payload.

- **Event-Driven Architecture**: Utilizes FreeRTOS Event Groups and Semaphores to synchronize tasks.

## Hardware Requirements

- ESP32-CAM Module: AI-Thinker variant.

- Micro-USB Cable: For programming and power.

- Wi-Fi Network: Required for MQTT communication.

## Software Requirements

- ESP-IDF (v5.1.4 or higher): Development framework for ESP32.

- MQTT Broker: A running MQTT broker (e.g., Mosquitto, AWS IoT, or Flespi).

- cJSON Library: For constructing JSON payloads.

- Base64 Library: For encode and decode data

## Project Structure

```bash
- ESP32_Camera_Node
- ├── main
- │   ├── main.c          # Main application logic
- │   ├── CMakeLists.txt
- │   ├── idf_component.yml 
- ├── lib
- │   ├── wifi_pro        # Wi-Fi configuration and initialization
- │   │  ├── wifi_pro.h
- │   │  ├── wifi_pro.c
- │   │  ├── CMakeLists.txt
- │   ├── server_cfg      # HTTP server configuration (if applicable)
- │   │  ├── server_cfg.h
- │   │  ├── server_cfg.c
- │   │  ├── CMakeLists.txt
- │   ├── mqtt_cfg        # MQTT configuration and client setup
- │   │  ├── mqtt_cfg.h
- │   │  ├── mqtt_cfg.c
- │   │  ├── CMakeLists.txt
- │   ├── project_system_files
- │   │  ├── project_system_files.h
- │   │  ├── project_system_files.c
- │   │  ├── CMakeLists.txt
- │   ├── nvs_storage
- │   │  ├── nvs_storage.h
- │   │  ├── nvs_storage.c
- │   │  ├── CMakeLists.txt
- │   ├── event_lib
- │   │  ├── shared.h
- │   │  ├── shared.c
- │   │  ├── CMakeLists.txt
- │   ├── client_cfg
- │   │  ├── client_cfg.h
- │   │  ├── client_cfg.c
- │   │  ├── CMakeLists.txt
- ├── spiffs_data
- │   ├── app.js
- │   ├── index.html
- │   ├── style.css
- ├── sdkconfig           # ESP-IDF configuration file
- ├── dependencies.lock
- ├── partition.csv
- └── README.md           # Project documentation
```

## Configuration

### Camera Configuration

- The camera is configured with the following pins and parameters:

```bash
#define CAM_PIN_PWDN 32
#define CAM_PIN_RESET -1
#define CAM_PIN_XCLK 0
#define CAM_PIN_SIOD 26
#define CAM_PIN_SIOC 27
#define CAM_PIN_D7 35
#define CAM_PIN_D6 34
#define CAM_PIN_D5 39
#define CAM_PIN_D4 36
#define CAM_PIN_D3 21
#define CAM_PIN_D2 19
#define CAM_PIN_D1 18
#define CAM_PIN_D0 5
#define CAM_PIN_VSYNC 25
#define CAM_PIN_HREF 23
#define CAM_PIN_PCLK 22

#define CAMERA_FRAME_SIZE FRAMESIZE_VGA
#define CAMERA_JPEG_QUALITY 10
```

### MQTT Configuration

- Broker URL: Set in mqtt_cfg.c

- Topic: Image data is published to /nodes/cameras/id_node

### Event Groups

- Event Groups are used for task synchronization:

- EVENT_CAMERA_INIT_DONE: Indicates that the camera initialization is complete.

- EVENT_CLIENT_POSTED: Signifies that the Wi-Fi and server setup are finished.

### Task Description

#### cameraTask

- Captures images using the ESP32-CAM module.

- Encoded with Base64

- Sends image after encode via MQTT in string format


#### MQTT Communication

- The pictureSend function in mqtt_cfg.c constructs the JSON payload and publishes it to the broker.

## Usage

### Clone the repository:

```bash
git clone https://github.com/hyutrn/ESP32_Node_Camera.git
cd ESP32_Node_Camera
```

### Configure the project:

```bash 
idf.py menuconfig
```
- Partition table: choose custom partition table CSV
- Serial flasher config: change Flash size from 2MB to 4MB
- Configure the MQTT broker settings.

### Build and flash the firmware:

```bash 
idf.py build
idf.py flash
```
### Monitor the device:

```bash
idf.py monitor
```
## Dependencies

- ESP-IDF (v5.1.4 or higher)

- cJSON library for JSON handling

- Troubleshooting

## Common Issues

- Camera Initialization Failed: Verify hardware connections and camera model.

- MQTT Connection Error: Ensure the broker URL, port, and credentials are correct.

## Debugging

- Use the ESP-IDF monitor to view logs and debug issues:

```bash
idf.py monitor
```

## License

- This project is licensed under the MIT License. See the LICENSE file for details.

## Acknowledgments

ESP-IDF documentation for its comprehensive resources.

AI-Thinker for the ESP32-CAM module.



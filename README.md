# STM32 Blackpill: Xsens MTi-1 IMU Driver & Telemetry Streamer

High-performance, non-blocking firmware developed for **STM32 (Blackpill)** to interface with the **Xsens MTi-1 IMU** via UART. It implements a robust Ring Buffer for asynchronous byte reception, XBus packet validation and parsing, and streams real-time sensor telemetry (**Accelerometer, Gyroscope, and Euler Angles**) over native **USB-CDC**.

---

## 🚀 Key Features

* **Non-Blocking Ring Buffer Reception:** Handles incoming UART byte streams safely via interrupt callback (`HAL_UART_RxCpltCallback`) without missing packets.
* **XBus Protocol Parser:** Automatically validates checksums, detects message IDs (`MID 0x36` MTData2), and extracts data groups based on XDI identifiers.
* **High-Rate Data Acquisition:** Configures the MTi-1 sensor outputs (ACC, GYR, Euler) to stream consistently at **100 Hz**.
* **USB-CDC Telemetry Stream:** Formats and transmits human-readable or parsable sensor strings over USB virtual COM port at a controlled 50 Hz rate (configurable).
* **System Health Indicator:** Built-in non-blocking LED heartbeat toggle every 200 ms.

---

## 🛠️ Hardware Stack & Connections

* **Microcontroller:** STM32 Blackpill (`STM32F401` / `STM32F411`)
* **IMU:** Xsens MTi-1 (configured in XBus protocol mode)
* **Connectivity:** Native USB (Virtual COM Port / CDC) connected to a PC or Single Board Computer.

### Pinout Configuration:
* **USART1 TX / RX:** Connected to the MTi-1 TX/RX lines (Baud rate: `115200`).
* **USB Pins (PA11 / PA12):** Native USB interface for CDC transmission.

---

## 📊 Data Frame Format (USB-CDC)

The microcontroller outputs a formatted string over the virtual serial port every 20ms:

```text
ACC: [ax, ay, az] | GYR: [gx, gy, gz] | EULER: [R: roll, P: pitch, Y: yaw] | test_flag: val\r\n

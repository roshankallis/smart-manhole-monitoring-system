# 🚧 IoT-Based Smart Manhole Monitoring System

## 📌 Project Overview

The **IoT-Based Smart Manhole Monitoring System** is a safety and monitoring solution designed to detect dangerous conditions inside or around manholes.

The system uses an **ESP8266 Wi-Fi microcontroller** along with multiple sensors to monitor:

- 🚨 Manhole cover tilting/opening
- 💧 Water level / flooding
- 💨 Harmful gas presence
- 📱 Real-time alerts through the Blynk IoT platform

When an abnormal condition is detected, the ESP8266 sends the sensor data to the **Blynk mobile/web dashboard**, allowing authorities or maintenance teams to monitor the manhole remotely.

---

## 🎯 Objectives

- Detect unauthorized or accidental opening/tilting of the manhole cover.
- Monitor water level inside the manhole.
- Detect harmful gases using an MQ-2 gas sensor.
- Provide real-time monitoring using IoT.
- Send alerts to the user through Blynk.
- Improve public safety and reduce accidents.
- Reduce the need for continuous manual inspection.

---

## 🧠 System Features

| Feature | Sensor/Module |
|---|---|
| Manhole tilt detection | Tilt Sensor |
| Water/flood detection | Float/Water Level Sensor |
| Gas detection | MQ-2 Gas Sensor |
| IoT communication | ESP8266 Wi-Fi |
| Mobile monitoring | Blynk IoT |
| Alert system | Blynk Notification |
| Power | 5V / Battery Supply |

---

## 🔧 Hardware Components

### Main Components

1. **ESP8266 NodeMCU**
2. **Tilt Sensor**
3. **Float/Water Level Sensor**
4. **MQ-2 Gas Sensor**
5. **Buzzer** *(optional)*
6. **LED indicators** *(optional)*
7. **5V Power Supply / Battery**
8. Connecting wires
9. Breadboard / PCB
10. Manhole prototype structure

---

## 💻 Software Requirements

- Arduino IDE
- ESP8266 Board Package
- Blynk IoT Platform
- C/C++ Programming
- Wi-Fi Network

---

## ⚙️ Working Principle

The system continuously monitors the condition of the manhole using multiple sensors.

### 1. Tilt Detection

The **tilt sensor** is installed with the manhole cover.

If the cover is tilted or moved beyond the normal position, the sensor changes its output state.

The ESP8266 detects this condition and sends an alert to the Blynk dashboard.

### 2. Water Level Detection

A **float/water level sensor** is used to detect rising water.

When the water reaches the sensor:

```text
Water Level Normal
       ↓
Sensor Monitoring
       ↓
Water Level Rises
       ↓
Sensor Activated
       ↓
ESP8266
       ↓
Blynk Alert

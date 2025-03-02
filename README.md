# esp32bluetoothdecoy

# 🚀 ESP32 Bluetooth Decoy

A simple **ESP32-WROOM-32U** project that **spoofs a fake Bluetooth device** by broadcasting a **custom BLE name, manufacturer data, and service UUID**.

---

## 📌 Features

✔ **Broadcasts a fake Bluetooth device name**  
✔ **Appears on BLE scanning apps (e.g., nRF Connect, LightBlue, etc.)**  
✔ **Can mimic AirPods, Smartwatches, Tile Trackers, etc.**  
✔ **Uses ESP32-WROOM-32U module**  
✔ **Runs autonomously after flashing**  

---

## 🛠️ Hardware Requirements

| Component            | Description |
|----------------------|-------------|
| **ESP32-WROOM-32U**  | Recommended for external antenna support |
| **Micro USB Cable**  | Used to flash the ESP32 |
| **Arduino IDE**      | Required for flashing |
| **BLE Scanner App**  | (e.g., nRF Connect, LightBlue, BLE Scanner) |

---

## 📥 Installation Steps

### 1️⃣ Install Arduino IDE & ESP32 Board Support
1. **Download & Install Arduino IDE**  
   - [Download Here](https://www.arduino.cc/en/software)

2. **Install ESP32 Board Support**
   - Open **Arduino IDE → Preferences**  
   - Add this URL under **"Additional Board Manager URLs"**:
     ```
     https://dl.espressif.com/dl/package_esp32_index.json
     ```
   - Go to **Tools → Board → Board Manager**  
   - Search for **ESP32** and install **"ESP32 by Espressif Systems"**.

---

### 2️⃣ Install Required Libraries
You'll need to install the **ESP32 BLE Library**:

1. Open **Arduino IDE → Library Manager**
2. Search for **ESP32 BLE Arduino**
3. Install the latest version.

---

### 3️⃣ Select the Correct Board
- **Board:** `ESP32-WROOM-DA Module`  
- **Partition Scheme:** `Huge APP (3MB No OTA)`  
- **Flash Mode:** `DIO`  
- **CPU Frequency:** `240 MHz`

---

### 4️⃣ Upload the Code
1. **Copy the Code Below** into **Arduino IDE**
2. **Select the correct board** (`ESP32-WROOM-DA Module`)
3. **Select the correct COM port** (under `Tools → Port`)
4. **Click Upload!** (Arrow Button)

---

## 📜 Code: ESP32 Bluetooth Spoofer

```cpp
#include <BLEDevice.h>
#include <BLEAdvertising.h>
#include <BLEUtils.h>

#define FAKE_DEVICE_NAME "AirPods_Pro"  // Change this to any Bluetooth name you want

void setup() {
    Serial.begin(115200);
    Serial.println("\n🔵 [SETUP] Simple Fake BLE Device Starting...");

    BLEDevice::init(FAKE_DEVICE_NAME);  // Set the fake Bluetooth name
    BLEServer* pServer = BLEDevice::createServer();
    BLEAdvertising* pAdvertising = pServer->getAdvertising();

    BLEAdvertisementData advData;
    advData.setFlags(0x06);
    advData.setName(FAKE_DEVICE_NAME);  // Make sure this name is the one being broadcast
    advData.setManufacturerData("0102030405");  // Fake manufacturer data
    advData.setAppearance(0x03C1);  // Generic headset or audio device

    BLEUUID serviceUUID((uint16_t)0x180D);  // Generic Heart Rate Service (Fake)
    advData.setCompleteServices(serviceUUID);

    pAdvertising->setAdvertisementData(advData);
    pAdvertising->setScanResponse(true);
    pAdvertising->start();

    Serial.println("✅ [BLE] Broadcasting Fake Bluetooth Device...");
    Serial.printf("🔵 [BLE] Fake Device Name: %s\n", FAKE_DEVICE_NAME);
}

void loop() {
    delay(1000);  // Keep running without interruptions
}
```
## 🛠️ How to Test
Open Serial Monitor (Set Baud Rate: 115200)

You should see output like this:

```
🔵 [SETUP] Simple Fake BLE Device Starting...
✅ [BLE] Broadcasting Fake Bluetooth Device...
🔵 [BLE] Fake Device Name: AirPods_Pro
Scan for Bluetooth Devices
```
Use nRF Connect (Android/iOS) or LightBlue (iOS).
Look for "AirPods_Pro" (or whatever name you set).

## ⚠️ Legal Disclaimer
This project is for educational and testing purposes only.
Using this tool in unauthorized environments may violate local laws and regulations.
🚨 Do not use this for malicious activities.

## 📌 FAQ
### ❓ Can I change the Bluetooth name?
Yes! Modify this line:

```
#define FAKE_DEVICE_NAME "AirPods_Pro"
```
Change "AirPods_Pro" to any Bluetooth device name.


### ❓ Can I use a different ESP32 model?
Yes, but ensure it supports BLE (Bluetooth Low Energy).

### ❓ How do I stop the script?
Simply unplug the ESP32 or upload a blank sketch.

## 📜 License
This project is licensed under the MIT License – Feel free to modify and share!

## 📡 Contributions
Want to improve the project? Fork it on GitHub and submit a pull request!

## 📌 Final Thoughts
🔥 You now have a working Bluetooth decoy that mimics real devices!
💡 Use this for security research, penetration testing, and education.
🚀 Let me know if you have questions or need enhancements.

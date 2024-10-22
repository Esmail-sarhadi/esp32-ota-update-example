

# 📶 ESP32 OTA Update Example

This project demonstrates how to set up an ESP32 for Over-The-Air (OTA) updates using the Arduino IDE. The ESP32 connects to a specified Wi-Fi network and can be updated wirelessly without needing a physical connection to a computer.

## 📑 Table of Contents
- [⚙️ Installation](#installation)
- [🚀 Usage](#usage)
- [📂 Project Structure](#project-structure)
- [🔧 Code Explanation](#code-explanation)
- [🐞 Troubleshooting](#troubleshooting)
- [🤝 Contributing](#contributing)
- [📄 License](#license)

## ⚙️ Installation

To set up this project, you need to have the Arduino IDE installed along with the ESP32 board support. You can install the necessary libraries using the Library Manager in the Arduino IDE.

## 🚀 Usage

1. Clone this repository:
    ```bash
    git clone https://github.com/esmail-sarhadi/esp32-ota-update-example.git
    ```
2. Navigate to the project directory:
    ```bash
    cd esp32-ota-update-example
    ```
3. Open the `ota_update.ino` file in the Arduino IDE.
4. Modify the `ssid` and `password` variables with your Wi-Fi credentials.
5. Upload the code to your ESP32 board.
6. Once the ESP32 is connected to the Wi-Fi network, you can perform OTA updates using the Arduino IDE.

## 📂 Project Structure

- `ota_update.ino`: Main script containing the code for Wi-Fi connection and OTA update setup.

## 🔧 Code Explanation

The code sets up the ESP32 for OTA updates as follows:

1. **Wi-Fi Connection:**
   ```cpp
   WiFi.mode(WIFI_STA);
   WiFi.begin(ssid, password);
   while (WiFi.waitForConnectResult() != WL_CONNECTED) {
     Serial.println(">> Connection Failed! Rebooting...");
     delay(5000);
     ESP.restart();
   }
   ```

2. **OTA Setup:**
   ```cpp
   ArduinoOTA.setHostname("MyESP32");
   ArduinoOTA.setPassword("admin");

   ArduinoOTA.onStart([]() {
     String type;
     if (ArduinoOTA.getCommand() == U_FLASH)
       type = "sketch";
     else
       type = "filesystem";
     Serial.println(">> Start updating " + type);
   });
   ArduinoOTA.onEnd([]() {
     Serial.println("\n >> End.");
   });
   ArduinoOTA.onProgress([](unsigned int progress, unsigned int total) {
     Serial.printf(">> Progress: %u%%\r", (progress / (total / 100)));
   });
   ArduinoOTA.onError([](ota_error_t error) {
     Serial.printf(">> Error[%u]: ", error);
     if (error == OTA_AUTH_ERROR) Serial.println(">> Auth Failed.");
     else if (error == OTA_BEGIN_ERROR) Serial.println(">> Begin Failed.");
     else if (error == OTA_CONNECT_ERROR) Serial.println(">> Connect Failed.");
     else if (error == OTA_RECEIVE_ERROR) Serial.println(">> Receive Failed.");
     else if (error == OTA_END_ERROR) Serial.println(">> End Failed.");
   });

   ArduinoOTA.begin();
   ```

3. **Handling OTA:**
   ```cpp
   void loop() {
     ArduinoOTA.handle();
   }
   ```

## 🐞 Troubleshooting

- **Connection Issues:** If the ESP32 fails to connect to the Wi-Fi, ensure that the SSID and password are correct and that the Wi-Fi network is working.
- **OTA Failures:** If OTA updates fail, check the Serial Monitor for error messages and ensure that the ESP32 is connected to the same network as your computer.

## 🤝 Contributing

Contributions are welcome! Please open an issue or submit a pull request for any changes.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📝 Description

This project showcases the process of configuring an ESP32 to accept OTA updates. By utilizing OTA, you can wirelessly update the firmware on your ESP32, making it convenient to deploy and maintain devices in the field.

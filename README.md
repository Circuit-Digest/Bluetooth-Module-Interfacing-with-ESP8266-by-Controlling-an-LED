# HC-05 Bluetooth Module Interfacing with ESP8266 - LED Control

![HC-05 Bluetooth ESP8266 Project](https://circuitdigest.com/sites/default/files/projectimage_mic/Bluetooth-Module-Interfacing-with-ESP8266.jpg)

## 📋 Project Overview

This project demonstrates how to interface the **HC-05 Bluetooth module** with the **ESP8266 NodeMCU** to wirelessly control an LED using Bluetooth communication. The project enables remote control of devices through a smartphone application, making it perfect for home automation and IoT applications.

**🔗 Detailed Tutorial:** [HC-05 Bluetooth Module Interfacing with ESP8266](https://circuitdigest.com/microcontroller-projects/hc-05-bluetooth-module-interfacing-with-esp8266-to-control-an-led)

## 🌟 Features

- **Wireless LED Control**: Control LED remotely via Bluetooth
- **Smartphone Integration**: Use any Bluetooth terminal app for control
- **Simple Commands**: Send 'B' to blink LED, 'S' to stop blinking
- **Non-blocking Code**: Uses `millis()` instead of `delay()` for better performance
- **Home Automation Ready**: Easily expandable to control relays and appliances
- **Cost-effective**: Uses affordable and readily available components

## 🛠️ Components Required

### Hardware
- [**NodeMCU ESP8266**](https://circuitdigest.com/microcontroller-projects/esp8266-nodemcu-with-atmega16-avr-microcontroller-to-send-an-email) - Main microcontroller with Wi-Fi capability
- [**HC-05 Bluetooth Module**](https://circuitdigest.com/microcontroller-projects/interfacing-hc-05-bluetooth-module-with-msp430-launchpad-to-control-an-led) - For wireless communication
- **Jumper Wires** - For connections
- **Breadboard** (optional) - For prototyping

### Software
- **Arduino IDE** - For programming the ESP8266
- **Serial Bluetooth Terminal** (Android App) - For sending commands

## 🔌 Circuit Wiring Diagram

### Pin Connections

| HC-05 Pin | ESP8266 NodeMCU Pin | Function |
|-----------|-------------------|----------|
| VCC | 3.3V or 5V | Power Supply |
| GND | GND | Ground Connection |
| TXD | D2 (GPIO4) | Serial Transmit |
| RXD | D3 (GPIO0) | Serial Receive |
| EN | Not Connected | Enable Pin (Optional) |
| STATE | Not Connected | Connection Status |

```
HC-05 Module    NodeMCU ESP8266
    VCC    →       3.3V/5V
    GND    →         GND
    TXD    →      D2 (GPIO4)
    RXD    →      D3 (GPIO0)
```

## 💻 Arduino Code

```cpp
/* HC-05 interfacing with NodeMCU ESP8266
   Author: Circuit Digest(circuitdigest.com)
*/
#include <SoftwareSerial.h>

SoftwareSerial btSerial(D2, D3); // Rx,Tx
int led = D4; // led also the internal led of NodemCU
int ledState = LOW; // led state to toggle
String ledB = "";
unsigned long previousMillis = 0; // millis instead of delay
const long interval = 500; // blink after every 500ms

void setup() {
  delay(1000);
  Serial.begin(9600);
  btSerial.begin(9600); // bluetooth module baudrate
  pinMode(D4, OUTPUT);
  Serial.println("Started...");
}

void loop() {
  if (btSerial.available() > 0) { // check if bluetooth module sends data
    char data = btSerial.read(); // read the data from HC-05
    switch (data)
    {
      case 'B': // if receive data is 'B'
        ledB = "blink"; // write the string
        break;
      case 'S': // if receive data is 'S'
        ledB = "stop";
        break;
      default:
        break;
    }
  }

  unsigned long currentMillis = millis();
  if (ledB == "blink") { // if received data is 'B' then start blinking
    Serial.println("blinking started");
    if (currentMillis - previousMillis >= interval) {
      previousMillis = currentMillis;
      if (ledState == LOW) {
        ledState = HIGH;
      } else {
        ledState = LOW;
      }
      digitalWrite(led, ledState);
    }
  }
}
```

## 🚀 Getting Started

### 1. Hardware Setup
1. Connect the HC-05 Bluetooth module to NodeMCU as per the wiring diagram
2. Ensure proper power connections (3.3V or 5V)
3. Double-check TX/RX cross-connections

### 2. Software Setup
1. Install Arduino IDE
2. Add ESP8266 board package to Arduino IDE
3. Select "NodeMCU 1.0 (ESP-12E Module)" as board
4. Upload the provided code to ESP8266

### 3. Bluetooth Pairing
1. Power on the circuit
2. Open Bluetooth settings on your smartphone
3. Search for HC-05 module (should appear as "HC-05")
4. Pair using default PIN: `1234` or `0000`

### 4. Testing
1. Download "Serial Bluetooth Terminal" app
2. Connect to HC-05 module in the app
3. Send command `B` to start LED blinking
4. Send command `S` to stop LED blinking

## 📱 Mobile App Usage

### Recommended Apps:
- **Serial Bluetooth Terminal** (Android)
- **Bluetooth Electronics** (Android/iOS)
- **ArduinoBluetooth** (Android)

### Commands:
- `B` - Start blinking LED
- `S` - Stop blinking LED

## 🔧 HC-05 Module Specifications

### Key Features:
- **Operating Voltage**: 3.3V to 5V
- **Communication**: UART (Serial)
- **Default Baud Rate**: 9600 bps
- **Range**: Up to 10 meters (open space)
- **Default PIN**: 1234 or 0000
- **Operating Modes**: Master, Slave, Loopback

### Pin Configuration:
- **VCC**: Power supply (3.3V-5V)
- **GND**: Ground reference
- **TXD**: Serial data output
- **RXD**: Serial data input  
- **EN**: Enable/Disable control
- **STATE**: Connection status indicator

## 🏠 Applications & Extensions

### Immediate Applications:
- **LED Control**: Basic on/off and blinking control
- **Home Automation**: Replace LED with relay for appliance control
- **Security Systems**: Remote monitoring and control
- **Educational Projects**: Learning Bluetooth communication

### Possible Extensions:
- Multiple device control
- Sensor data monitoring
- Integration with IoT platforms
- Voice control integration
- Advanced home automation systems

## 🛠️ Troubleshooting

| Issue | Possible Cause | Solution |
|-------|---------------|----------|
| HC-05 not discoverable | Power supply issue | Check 3.3V/5V connections and LED indicator |
| Cannot pair with phone | Wrong PIN code | Try "1234", "0000", or reset module |
| No data transmission | Wrong wiring | Check TX/RX cross-connections |
| LED not responding | Code or hardware issue | Test with Serial Monitor first |
| Module not responding | Baud rate mismatch | Ensure 9600 baud rate in code and module |

## ❓ FAQ

### Q1: Can I use HC-06 instead of HC-05?
**A:** Yes, HC-06 works the same way but only supports slave mode, while HC-05 supports both master and slave modes.

### Q2: What's the maximum Bluetooth range?
**A:** The HC-05 module typically has a range of 10 meters in open space. Obstacles and interference may reduce range.

### Q3: Why use SoftwareSerial instead of HardwareSerial?
**A:** ESP8266's hardware serial is used for programming and debugging. SoftwareSerial provides an additional serial port for HC-05 communication.

### Q4: Can I control multiple devices?
**A:** Yes, you can control multiple LEDs, relays, or sensors by adding more switch-case logic for different command characters.

## 📚 Related Projects

- [Capacitive Touch Sensor Home Automation using ESP32](https://circuitdigest.com/)
- [ESP32 BLE Server - GATT Service for Battery Level](https://circuitdigest.com/)
- [Bluetooth Controlled Robot using Arduino](https://circuitdigest.com/)
- [Android Controlled Home Automation](https://circuitdigest.com/)
- [ESP8266](https://circuitdigest.com/esp8266-projects)

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss the changes you would like to make.

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

## 📞 Support

If you have any questions or need help with the project:
- 🌐 Visit: [Circuit Digest Forum](https://circuitdigest.com/forum)
- 📧 Comment on the [original tutorial](https://circuitdigest.com/microcontroller-projects/hc-05-bluetooth-module-interfacing-with-esp8266-to-control-an-led)

## 🔗 References

- [Original Tutorial](https://circuitdigest.com/microcontroller-projects/hc-05-bluetooth-module-interfacing-with-esp8266-to-control-an-led)
- [ESP8266 Documentation](https://arduino-esp8266.readthedocs.io/)
- [HC-05 Module Datasheet](https://www.electronicwings.com/)

---

**⭐ If this project helped you, please give it a star!**

**🔄 Don't forget to follow for more exciting IoT and Arduino projects!**

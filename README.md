# ESP32 WiFi-Controlled Robotic Arm 🦾

A web-based robotic arm control system using ESP32 with WiFi connectivity, featuring real-time servo control, movement recording, and playback capabilities.

## 🎯 Features

- **WiFi Control**: Control the robotic arm wirelessly through a web interface
- **Real-time Control**: Adjust 4 servo motors simultaneously via sliders
- **Record & Playback**: Record complex movement sequences and replay them
- **Responsive Web UI**: Clean, modern interface accessible from any device
- **WebSocket Communication**: Low-latency bidirectional communication
- **No Internet Required**: Creates its own WiFi access point

## 📋 Overview

This project creates a WiFi-controlled 4-DOF (Degrees of Freedom) robotic arm using an ESP32 microcontroller. The system features:
- Web-based control interface (no app installation needed)
- Independent control of Base, Shoulder, Elbow, and Gripper
- Movement recording with precise timing
- Automated playback of recorded sequences

## 🔧 Hardware Requirements

### Electronics
- **ESP32 Development Board** (ESP32-WROOM-32 or similar)
- **4 Servo Motors** (SG90 or MG996R recommended)
  - Base servo (180° rotation)
  - Shoulder servo (180° rotation)
  - Elbow servo (180° rotation)
  - Gripper servo (180° rotation)
- **External Power Supply** (5V, 2-4A depending on servos)
- **Breadboard and Jumper Wires**
- **USB Cable** for programming

### Mechanical (Optional)
- Robotic arm chassis/frame
- Servo mounting brackets
- Gripper mechanism
- Base platform

## 📚 Software Requirements

### Arduino IDE Setup
```
1. Arduino IDE 1.8.x or 2.x
2. ESP32 Board Support
3. Required Libraries:
   - ESP32Servo
   - AsyncTCP
   - ESPAsyncWebServer
   - WiFi (built-in)
```

### Installing ESP32 Board Support

1. Open Arduino IDE
2. Go to `File → Preferences`
3. Add to "Additional Board Manager URLs":
```
   https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
```
4. Go to `Tools → Board → Boards Manager`
5. Search for "ESP32" and install

### Installing Required Libraries

**Method 1: Arduino Library Manager**
1. Go to `Sketch → Include Library → Manage Libraries`
2. Search and install:
   - `ESP32Servo` by Kevin Harrington
   - `AsyncTCP` by Me-No-Dev
   - `ESPAsyncWebServer` by Me-No-Dev

**Method 2: Manual Installation**
Download and install from GitHub:
- [ESP32Servo](https://github.com/madhephaestus/ESP32Servo)
- [AsyncTCP](https://github.com/me-no-dev/AsyncTCP)
- [ESPAsyncWebServer](https://github.com/me-no-dev/ESPAsyncWebServer)

## 🔌 Hardware Wiring

### ESP32 Pin Connections

| Component | ESP32 Pin | Wire Color |
|-----------|-----------|------------|
| **Base Servo** | GPIO 27 | Signal (Orange/Yellow) |
| **Shoulder Servo** | GPIO 26 | Signal (Orange/Yellow) |
| **Elbow Servo** | GPIO 25 | Signal (Orange/Yellow) |
| **Gripper Servo** | GPIO 33 | Signal (Orange/Yellow) |

### Power Connections
```
Servo Power (All 4 Servos):
├── Red Wire    → External 5V Power Supply (+)
├── Brown Wire  → External 5V Power Supply (-)
└── Orange Wire → ESP32 GPIO Pin (see table above)

ESP32:
├── GND → External Power Supply Ground (COMMON GROUND!)
└── VIN → 5V (if powering ESP32 from external supply)
```

### Wiring Diagram (Text)
```
External 5V Power Supply
    │
    ├──[+5V]───┬─→ Servo 1 (Red)
    │          ├─→ Servo 2 (Red)
    │          ├─→ Servo 3 (Red)
    │          └─→ Servo 4 (Red)
    │
    └──[GND]───┬─→ Servo 1 (Brown)
               ├─→ Servo 2 (Brown)
               ├─→ Servo 3 (Brown)
               ├─→ Servo 4 (Brown)
               └─→ ESP32 GND (IMPORTANT!)

ESP32
    ├─ GPIO 27 ──→ Base Servo (Orange)
    ├─ GPIO 26 ──→ Shoulder Servo (Orange)
    ├─ GPIO 25 ──→ Elbow Servo (Orange)
    └─ GPIO 33 ──→ Gripper Servo (Orange)
```

⚠️ **CRITICAL**: 
- Always connect ESP32 GND to power supply GND (common ground)
- Never power servos from ESP32's 3.3V or 5V pins
- Use external power supply rated for your servos' total current draw

## 🚀 Installation & Setup

### Step 1: Upload Code to ESP32

1. Open Arduino IDE
2. Copy the provided code into a new sketch
3. Configure board settings:
```
   Tools → Board → ESP32 Arduino → ESP32 Dev Module
   Tools → Flash Size → 4MB
   Tools → Upload Speed → 115200
```
4. Connect ESP32 via USB
5. Select correct port: `Tools → Port → (your ESP32 port)`
6. Click Upload

### Step 2: Configure WiFi (Optional)

Default settings in code:
```cpp
const char* ssid = "RobotArm";        // Change WiFi name here
const char* password = "12345678";     // Change password here (min 8 chars)
```

### Step 3: Power Up

1. Connect servos according to wiring diagram
2. Connect external power supply
3. Power on ESP32
4. Open Serial Monitor (115200 baud) to see IP address

## 🎮 Usage

### Connecting to Robot Arm

1. **Power on the ESP32**
2. **Connect to WiFi**:
   - Network Name: `RobotArm`
   - Password: `12345678`
3. **Open web browser** on connected device
4. **Navigate to**: `http://192.168.4.1`
5. **Control interface loads automatically**

### Web Interface Controls

#### Servo Control Sliders

| Control | Range | Function |
|---------|-------|----------|
| **Gripper** | 0-180° | Opens/closes gripper |
| **Elbow** | 0-180° | Raises/lowers arm tip |
| **Shoulder** | 0-180° | Raises/lowers arm mid-section |
| **Base** | 0-180° | Rotates entire arm left/right |

#### Recording Movements

1. Click **"Record"** button (turns GREEN)
2. Move sliders to create your sequence
3. Each slider movement is recorded with timing
4. Click **"Record"** again to stop (turns BLUE)

#### Playing Back Movements

1. Ensure you have recorded a sequence
2. Click **"Play"** button (turns GREEN)
3. Arm moves to starting position slowly
4. Recorded sequence plays automatically
5. Click **"Play"** again to stop (turns BLUE)

### Visual Feedback

- **Blue Buttons**: OFF/Inactive state
- **Green Buttons**: ON/Active state
- **Disabled Sliders**: Grey appearance (during playback)

## ⚙️ Configuration & Customization

### Adjusting Servo Pins

Edit the `servoPins` vector:
```cpp
std::vector servoPins = 
{
  { Servo(), 27 , "Base", 90},      // Change pin, name, or initial position
  { Servo(), 26 , "Shoulder", 90},
  { Servo(), 25 , "Elbow", 90},
  { Servo(), 33 , "Gripper", 90},
};
```

### Changing Initial Positions

Modify the last value in each servo definition:
```cpp
{ Servo(), 27 , "Base", 120},  // Starts at 120° instead of 90°
```

### Adjusting Playback Speed

In `playRecordedRobotArmSteps()` function:
```cpp
delay(50);  // Change this value (milliseconds)
            // Lower = faster movement
            // Higher = slower, smoother movement
```

### Modifying WiFi Settings
```cpp
const char* ssid = "MyRobotArm";      // Your custom network name
const char* password = "MyPassword";   // Your custom password (8+ chars)
```

### Customizing Web Interface Colors

In the HTML section, modify CSS:
```css
#4A90E2  /* Primary blue - buttons and sliders */
#357ABD  /* Hover blue */
#50E3C2  /* Secondary cyan - headings */
```

## 🔧 Troubleshooting

### ESP32 Not Creating WiFi Network

**Problem**: Cannot see "RobotArm" WiFi network

**Solutions**:
```
1. Check Serial Monitor for IP address confirmation
2. Verify ESP32 is powered properly
3. Reset ESP32 and wait 10 seconds
4. Check if WiFi is enabled on your device
5. Try different WiFi channel:
   WiFi.softAP(ssid, password, 6); // Channel 6
```

### Cannot Access Web Interface

**Problem**: 192.168.4.1 doesn't load

**Solutions**:
```
1. Verify you're connected to "RobotArm" WiFi
2. Check Serial Monitor for actual IP address
3. Try: http://192.168.4.1 (not https)
4. Clear browser cache
5. Try different browser (Chrome recommended)
6. Disable cellular data (mobile devices)
```

### Servos Not Moving

**Problem**: Sliders move but servos don't respond

**Solutions**:
```
1. Check external power supply is ON and adequate
2. Verify all servo signal wires connected correctly
3. Ensure common ground between ESP32 and power supply
4. Test servos individually with simple code
5. Check servo power consumption vs supply capacity
6. Verify servo pins in code match physical connections
```

### Servos Jitter or Behave Erratically

**Problem**: Servos vibrate or move unexpectedly

**Solutions**:
```
1. Ensure stable power supply (sufficient current)
2. Add capacitor (100-1000μF) across power supply
3. Keep signal wires away from power wires
4. Use shorter, shielded wires if possible
5. Check for loose connections
6. Reduce number of simultaneous servo movements
```

### Web Interface Laggy or Unresponsive

**Problem**: Slow response to slider movements

**Solutions**:
```
1. Move closer to ESP32 for better WiFi signal
2. Disconnect other devices from WiFi network
3. Reduce WebSocket message frequency in code
4. Clear browser cache
5. Use wired connection for initial setup
```

### Recording Doesn't Work

**Problem**: Recorded movements don't play back correctly

**Solutions**:
```
1. Ensure Record button turns GREEN before moving
2. Stop recording before clicking Play
3. Don't refresh page after recording
4. Check Serial Monitor for memory issues
5. Keep recordings under 100 steps for ESP32 memory
```

### Upload Errors

**Problem**: Code won't upload to ESP32

**Solutions**:
```
1. Hold "BOOT" button while uploading
2. Check correct board selected in Tools menu
3. Try different USB cable (data cable, not charge-only)
4. Lower upload speed: Tools → Upload Speed → 115200
5. Install CP210x or CH340 drivers if needed
```

## 📊 System Architecture
```
┌─────────────────┐
│  Web Browser    │
│  (Any Device)   │
└────────┬────────┘
         │ HTTP/WebSocket
         │ WiFi
         ▼
┌─────────────────┐
│     ESP32       │
│  ┌───────────┐  │
│  │ Web Server│  │
│  │ WebSocket │  │
│  └─────┬─────┘  │
│        │        │
│  ┌─────▼─────┐  │
│  │  Servo    │  │
│  │ Controller│  │
│  └─────┬─────┘  │
└────────┼────────┘
         │ PWM Signals
         ▼
┌────────────────────┐
│   4 Servo Motors   │
│  Base │ Shoulder   │
│  Elbow │ Gripper    │
└────────────────────┘
```

## 🎓 How It Works

### WiFi Access Point Mode

1. ESP32 creates its own WiFi network (AP mode)
2. Devices connect directly to ESP32
3. No internet or router required
4. Default IP: 192.168.4.1

### WebSocket Communication

1. **Client → ESP32**: Slider changes send servo positions
2. **ESP32 → Client**: Current servo states and button status
3. **Real-time**: Bidirectional updates with minimal latency
4. **Persistent**: Connection maintained throughout session

### Movement Recording System
```cpp
Recording Process:
1. Record button pressed → Clear previous recording
2. Initial positions captured for all servos
3. Each slider movement stored with:
   - Servo index (which servo)
   - Target angle (0-180°)
   - Time delay since last movement
4. Record button pressed again → Stop recording

Playback Process:
1. Play button pressed
2. Arm slowly moves to initial position
3. 2-second delay
4. Recorded movements executed with original timing
5. WebSocket updates sliders in real-time
```

### Servo Control Flow
```
User Input (Web Slider)
    ↓
WebSocket Message
    ↓
Parse Key-Value Pair
    ↓
writeServoValues()
    ↓
├─ If Recording: Store step with timing
└─ Write angle to servo
    ↓
Servo Moves
```

## 🔐 Security Notes

- Default WiFi password is weak - **change for production use**
- Access point is open to anyone in range
- No authentication on web interface
- Consider adding password protection for sensitive applications
- Keep firmware updated

## 📈 Advanced Features & Modifications

### Adding More Servos
```cpp
// Add to servoPins vector:
{ Servo(), 32 , "Wrist", 90},  // GPIO 32, starts at 90°

// Add slider to HTML:

  Wrist:
  
    
  


// Add case in WebSocket handler:
else if (key == "Wrist")
{
  writeServoValues(4, valueInt);  // Index 4 for 5th servo
}
```

### Adding Speed Control

Add global speed variable and modify playback:
```cpp
int playbackSpeed = 100; // 100% normal speed

// In playback function:
delay(recordedStep.delayInStep * playbackSpeed / 100);
```

### Adding Position Presets
```cpp
void moveToPreset(int presetNumber) {
  switch(presetNumber) {
    case 1: // Home position
      servoPins[0].servo.write(90);
      servoPins[1].servo.write(90);
      servoPins[2].servo.write(90);
      servoPins[3].servo.write(90);
      break;
    case 2: // Pickup position
      servoPins[0].servo.write(45);
      servoPins[1].servo.write(120);
      // ... etc
  }
}
```

### Memory Optimization

For longer recordings:
```cpp
// Use SPIFFS or SD card for storage
#include 

void saveRecording() {
  File file = SPIFFS.open("/recording.dat", FILE_WRITE);
  // Write recordedSteps to file
}
```

## 🛠️ Hardware Assembly Tips

1. **Mount servos securely** - vibration causes errors
2. **Use proper connectors** - don't rely on breadboard for power
3. **Cable management** - keep signal wires away from motors
4. **Test individually** - verify each servo before assembly
5. **Balance the arm** - center of gravity affects performance
6. **Gradual assembly** - add one servo at a time and test

## 📝 Code Structure
```
main.ino
├── Global Variables
│   ├── ServoPins struct & vector
│   ├── RecordedStep struct & vector
│   └── WiFi credentials
│
├── HTML Interface (embedded)
│   ├── CSS styling
│   └── JavaScript WebSocket handling
│
├── setup()
│   ├── Initialize servos
│   ├── Start WiFi AP
│   ├── Configure web server
│   └── Setup WebSocket handlers
│
└── loop()
    ├── Clean WebSocket clients
    └── Handle playback if active

Functions:
├── handleRoot() - Serve web page
├── onRobotArmInputWebSocketEvent() - Process commands
├── writeServoValues() - Move servo & record
├── playRecordedRobotArmSteps() - Playback sequence
└── sendCurrentRobotArmState() - Sync with clients
```

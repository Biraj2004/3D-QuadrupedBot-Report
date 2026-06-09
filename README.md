# 3D Printing & Additive Manufacturing: Quadruped Robot Development Report

This repository contains the LaTeX source code, compiled technical report, mechanical blueprints, wiring schematics, and development guidelines for the **8-DOF Quadruped Robot Project** developed by Group-E, Department of Electronics and Communication Engineering (ECE) at Cooch Behar Government Engineering College (CGEC) in 2026. This project was conducted as part of the certification-based industrial training program under the **WTL & MeitY** (Ministry of Electronics and Information Technology, Govt. of India) initiative.

---

## 📂 Repository Directory Structure

```plaintext
3D-QuadrupedBot-Report/
├── 3D_PROJECT_REPORT.tex              # Full XeLaTeX report source code (uses vspace placeholders)
├── 3D_PROJECT_REPORT.pdf              # Compiled XeLaTeX PDF report (blank slots ready for images)
├── 3D_PROJECT_REPORT.toc              # Generated Table of Contents index file
├── README.md                          
├── build_guide/                       # Build instructions, specifications, and references
│   ├── 3D_PROJECT_REPORT_BUILD_GUIDE.md  
│   ├── QUADRUPED ROBOT PROJECT - DEVELOPMENT INSTRUCTIONS.txt
│   └── previous batch project (3d) copy.pdf  
└── images/                            # Technical schematics and photographic assets
    ├── CGEC-Logo-colorful.jpg         
    ├── cad_assembly.jpg               
    ├── esp32_pca9685_wiring_setup.jpg 
    ├── fig 1.0.jpeg                   
    ├── fig1.1.jpeg                    
    ├── our-bot.jpeg                   
    └── quadruped_robot_blueprint.jpg  
```

---

## 🤖 Robot Specifications & Design Aesthetics

The physical quadruped robot is designed and fabricated with specific material tolerances and color coordinates matching the physical prototype:
* **Main Chassis (Core Plate)**: Black textured 3D-printed body (PLA+).
* **Limbs (Legs)**: 4 bright pink/magenta jointed leg segments (PETG) to absorb dynamic impacts.
* **Head Box**: Bright blue rectangular enclosure housing the electronics, featuring dual circular ultrasonic sensor "eyes" on the front panel.
* **Actuators**: 8 blue micro servo motors (SG90/MG90S type) for leg joints, and 1 additional servo for head rotation.
* **Physical Dimensions**:
  * **Core Plate**: $110 \times 85 \times 4\text{ mm}$
  * **Head Box**: $85 \times 65 \times 45\text{ mm}$ (Wall thickness: $2.5\text{ mm}$)
  * **Upper Leg**: $45 \times 10 \times 5\text{ mm}$
  * **Lower Leg**: $55 \times 10 \times 5\text{ mm}$

---

## ⚡ Electronics & Wiring Architecture

The robot utilizes an **ESP32 DevKit V1** as the master controller and a **PCA9685 16-channel 12-bit PWM driver** as the servo interface via the I2C bus.

### 1. ESP32 to PCA9685 Control Connections
| ESP32 Pin | PCA9685 Pin | Description |
| :--- | :--- | :--- |
| **3.3V** | **VCC** | Logic Power |
| **GND** | **GND** | Shared Logic Ground |
| **GPIO21** | **SDA** | I2C Serial Data Line |
| **GPIO22** | **SCL** | I2C Serial Clock Line |

### 2. Servo Power Configuration
> [!IMPORTANT]
> The PCA9685 logic VCC does **not** power the servo motor outputs. An external power supply must be connected to the terminal block to drive the servos.

| External Power Source | PCA9685 Pin | Description |
| :--- | :--- | :--- |
| **+5V Battery Pack** | **V+ (Terminal)** | Servo Power Rail (4.8V – 5.2V) |
| **Battery GND** | **GND (Terminal)** | High-Current Common Ground |

### 3. Servo Channel Mapping
| Channel | Joint/Part | Locomotion Type |
| :---: | :--- | :--- |
| **CH0** | Front Left Hip | Hip Actuator |
| **CH1** | Front Left Knee | Knee Actuator |
| **CH2** | Front Right Hip | Hip Actuator |
| **CH3** | Front Right Knee | Knee Actuator |
| **CH4** | Rear Left Hip | Hip Actuator |
| **CH5** | Rear Left Knee | Knee Actuator |
| **CH6** | Rear Right Hip | Hip Actuator |
| **CH7** | Rear Right Knee | Knee Actuator |
| **CH8** | Head Rotation | Head Yaw Control |

### 🔍 Critical Debugging Discovery
During initial verification, the servo motors remained completely unresponsive even though the I2C scan successfully identified the PCA9685 at address `0x40`. Using a multimeter, the team measured the PCA9685 **V+ servo rail at only 0.10V** (expected ~5.0V). The issue was traced to the lack of an external 5V connection to the screw terminals. Connecting a dedicated 5V external battery pack directly to the V+ terminal resolved the issue.

---

## ⚙️ 3D Printing & Materials System

Additive manufacturing parameters were optimized to balance structural rigidity with weight distribution:
* **Materials Used**:
  * **PLA+ (Enhanced Polylactic Acid)**: Used for the Core Plate, Head Box, and Servo Mounts due to its higher stiffness, low warp, and dimensional accuracy.
  * **PETG (Polyethylene Terephthalate Glycol)**: Used for the Upper and Lower Legs to provide impact resistance, flexibility, and strong layer-to-layer adhesion under mechanical stress.
* **Standard Slicer Settings**:
  * **Layer Height**: $0.2\text{ mm}$ (balanced resolution)
  * **Infill Density**: $40\%$
  * **Infill Pattern**: *Grid* (Core Plate and Head Box for rigid load-bearing); *Lines* (Leg segments to reduce moving mass)
  * **Perimeter Walls**: $4$
  * **Nozzle Diameter**: $0.4\text{ mm}$
  * **Print Speed**: $50\text{ mm/s}$

---

## 💻 Firmware Code Examples (Arduino IDE)

### 1. Direct Single Servo Test Code (GPIO18)
Used for calibrating and testing individual SG90 servos before connecting them to the PCA9685 driver.
```cpp
#include <ESP32Servo.h>

Servo myServo;

void setup() {
  myServo.attach(18); // Attach signal wire to ESP32 GPIO18
}

void loop() {
  myServo.write(0);     // Sweep to 0 degrees
  delay(1000);
  myServo.write(90);    // Sweep to 90 degrees (neutral position)
  delay(1000);
  myServo.write(180);   // Sweep to 180 degrees
  delay(1000);
}
```

### 2. PCA9685 I2C Address Scanner
Scans the ESP32's I2C lines to confirm physical hardware communication with the PCA9685.
```cpp
#include <Wire.h>

void setup() {
  Wire.begin(21, 22); // SDA = GPIO21, SCL = GPIO22
  Serial.begin(115200);
  Serial.println("Scanning I2C bus...");
  for (byte addr = 1; addr < 127; addr++) {
    Wire.beginTransmission(addr);
    if (Wire.endTransmission() == 0) {
      Serial.print("I2C device found at 0x");
      if (addr < 16) Serial.print("0");
      Serial.println(addr, HEX);
    }
  }
  Serial.println("Scan complete.");
}

void loop() {}
```
*Expected Serial Monitor Output:* `I2C device found at 0x40` (Factory default address for PCA9685).

### 3. PCA9685 All-Channel Servo Sweep
Sweeps all 9 channels simultaneously once the V+ power rail issue is resolved.
```cpp
#include <Wire.h>
#include <Adafruit_PWMServoDriver.h>

Adafruit_PWMServoDriver pwm = Adafruit_PWMServoDriver(0x40);

#define SERVOMIN  150   // Minimum pulse length out of 4096 (~0 degrees)
#define SERVOMAX  600   // Maximum pulse length out of 4096 (~180 degrees)
#define SERVO_FREQ 50   // Analog servos run at 50Hz updates

uint16_t angleToPulse(int angle) {
  return map(angle, 0, 180, SERVOMIN, SERVOMAX);
}

void setup() {
  Wire.begin(21, 22);
  pwm.begin();
  pwm.setOscillatorFrequency(27000000);
  pwm.setPWMFreq(SERVO_FREQ);
  delay(10);
}

void loop() {
  for (int ch = 0; ch < 9; ch++) {
    pwm.setPWM(ch, 0, angleToPulse(90));   // Move all servos to neutral
  }
  delay(2000);
  for (int ch = 0; ch < 9; ch++) {
    pwm.setPWM(ch, 0, angleToPulse(45));   // Move all servos to 45 degrees
  }
  delay(2000);
}
```

---

## 📈 Development Roadmap & Status

The project progresses through a 9-phase lifecycle:

| Phase | Description | Status | Verification Notes |
| :---: | :--- | :---: | :--- |
| **1** | Component verification | **Complete** | ESP32, PCA9685 address, and direct servo sweeps verified. |
| **2** | CAD design in FreeCAD | **Complete** | Parametric models for Core Plate, legs, and Head Box completed. |
| **3** | 3D printing all parts | **Complete** | Print jobs in PLA+ and PETG completed in ECE 3D Printing Lab. |
| **4** | Servo calibration | **Complete** | Programmed neutral trim offsets for all 9 actuators. |
| **5** | Standing pose | **Complete** | Calibrated center of mass and leg joints for structural stability. |
| **6** | Walking gait | **In Progress** | Inverse kinematics and walking sequences are currently being tested. |
| **7** | Turning gait | *Planned* | Implementing differential rotation of the hip actuators. |
| **8** | Sensor integration | *Planned* | Hooking up front HC-SR04 ultrasonic sensors and IMUs. |
| **9** | Autonomous navigation | *Planned* | Obstacle avoidance routines using real-time sensor loops. |

---

## 🛠️ LaTeX Report Compilation Instructions

The formal project report [3D_PROJECT_REPORT.tex](3D_PROJECT_REPORT.tex) is written using **XeLaTeX** to support custom fonts and Devanagari Unicode scripts.

### How to Compile Manually
To resolve internal citations, references, and the Table of Contents, run the compiler twice:
```powershell
xelatex 3D_PROJECT_REPORT.tex
xelatex 3D_PROJECT_REPORT.tex
```

### Inserting Technical Diagram Assets into the PDF
To insert high-resolution JPEGs into the blank placeholder slots in the compiled PDF without causing compatibility errors:
1. Open the compiled [3D_PROJECT_REPORT.pdf](3D_PROJECT_REPORT.pdf) in **Adobe Acrobat**.
2. Click the **Edit PDF** tool on the right-hand panel.
3. Select **Add Image** from the top menu toolbar.
4. Browse and select the corresponding image from the `images/` directory per the mapping table below.
5. Align the image inside the empty space reserved by the `\vspace{10cm}` command, directly above the caption.
6. Save the PDF.

### Image Mapping Table
| Target Placement | Image File | Description / Caption |
| :--- | :--- | :--- |
| **Cover Page Slot** | `images/our-bot.jpeg` | Physical 8-DOF quadruped robot prototype |
| **Title Page Slot** | `images/CGEC-Logo-colorful.jpg` | Official CGEC emblem |
| **Figure 1 (Intro)** | `images/fig 1.0.jpeg` | A Quadruped Robot (reference photo) |
| **Figure 2 (Electronics)**| `images/esp32_pca9685_wiring_setup.jpg` | Landscape ESP32 & PCA9685 wiring setup |
| **Figure 6 (CAD)** | `images/cad_assembly.jpg` | Dark-mode ideaMaker grid assembly view |
| **Figure 10 (Printed)** | `images/fig1.1.jpeg` | Photo of fabricated PETG leg components |
| **Figure 12 (Results)** | `images/quadruped_robot_blueprint.jpg` | 2:1 Side-by-side dimensioned blueprint |

---

## 👥 Group E Members & Guidance (CGEC ECE 2027)

### Group Members
1. **SARUPYA GUHA** (Roll No.: 34900323046 / Reg. ID: FS08A0198)
2. **SOHAN GHOSH** (Roll No.: 34900323050 / Reg. ID: FS08A0202)
3. **SAYAN SARKAR** (Roll No.: 34900323048 / Reg. ID: FS08A0157)
4. **DEBANJAN CHAKRABORTY** (Roll No.: 34900323015 / Reg. ID: FS08A0154)
5. **JOYDIP DAS** (Roll No.: 34900323020 / Reg. ID: FS08A0220)
6. **AUSHI SARKAR** (Roll No.: 34900323011 / Reg. ID: FS08A0288)
7. **ANUPAM DUTTA** (Roll No.: 34900323003 / Reg. ID: FS08A0206)
8. **BIKKRAM DAS** (Roll No.: 34900323012 / Reg. ID: FS08A0528)
9. **BIRAJ SARKAR** (Roll No.: 34900323013 / Reg. ID: FS08A0239)
10. **SOUMYAJIT PANDIT** (Roll No.: 34900323052 / Reg. ID: FS08A0189)

### Faculty & Supervisors (WTL & MeitY Project Panel)
* **Dr. PALASH DAS** — HOD, Department of Electronics and Communication Engineering, CGEC
* **Mrs. SUSMITA BANIK BARIK** — Project Engineer, WTL & MeitY Project
* **Mr. ARIJIT BHUNIA** — Project Co-ordinator, WTL & MeitY Project
* **Mr. BHASKAR MONDAL** — Faculty, WTL & MeitY Project

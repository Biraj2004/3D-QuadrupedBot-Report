# Quadruped Robot Development Report & Technical Assets

This repository contains the LaTeX source code, compiled report document, and high-quality technical diagrams for the **8-DOF Quadruped Robot Project** developed by Group-E, Department of Electronics and Communication Engineering (ECE) at Cooch Behar Government Engineering College (CGEC), 2026. This project was conducted under the **WTL & MeitY** industrial training certification program.

---

## 📂 Directory Structure

```plaintext
3D-Report/
├── 3D_PROJECT_REPORT.tex       # XeLaTeX report source code (using vspace placeholders)
├── 3D_PROJECT_REPORT.pdf       # Compiled XeLaTeX PDF report (blank slots ready for images)
├── README.md                   # Project documentation (this file)
├── student_details.txt         # Group member names, rolls, and registration IDs
├── build_guide/                # Build requirements, batch references, and instructions
└── images/                     # Technical schematics and photographic assets
    ├── our-bot.jpeg            # Reference photo of the physical quadruped robot
    ├── esp32_pca9685_wiring_setup_simple.svg  # Source landscape wiring SVG
    ├── esp32_pca9685_wiring_setup.jpg        # High-res wiring JPEG (safe, no logic short circuits)
    ├── quadruped_robot_blueprint_simple.svg  # Source landscape blueprint SVG
    ├── quadruped_robot_blueprint.jpg        # High-res robot blueprint JPEG (Front & Side view)
    ├── cad_assembly.jpg        # High-res ideaMaker assembly screenshot matching bot colors
    └── ...                     # Other workspace assets
```

---

## 🤖 Robot Specifications (Reference Photo Match)

The generated assets are customized to perfectly match the color scheme of the physical robot constructed in the lab (as seen in `images/our-bot.jpeg`):
* **Main Chassis**: Black textured 3D-printed body (PLA+).
* **Limbs**: 4 bright pink/magenta jointed leg segments (PETG).
* **Head Box**: Bright blue rectangular enclosure housing the electronics, featuring dual circular ultrasonic sensor eyes on the front.
* **Actuators**: 8 blue micro servo motors (SG90/MG90S type) with standard signal/power wiring.

---

## 🖼️ Technical Diagram Assets

### ⚡ 1. ESP32 & PCA9685 Wiring Schematic
* **File**: `images/esp32_pca9685_wiring_setup.jpg`
* **Features**: Represents the connections between the ESP32 DevKit V1, PCA9685 16-channel driver, a 6V battery pack, and a servo motor on CH0.
* **Correction**: Rerouted battery positive and negative lines vertically through $y=70$ and $y=75$ respectively to cleanly enter the center of the screw terminals ($y=37.5$), avoiding overlapping logic VCC rails ($y=80$) and preventing short-circuit representations.

### 📐 2. Technical Robot Blueprint
* **File**: `images/quadruped_robot_blueprint.jpg`
* **Features**: A landscape 2:1 side-by-side view (Left Side View and Front View) with dimension ticks ($11\text{cm}$ height, $22\text{cm}$ length, $15\text{cm}$ span).
* **Correction**: Repositioned the front view head block elements to completely separate the ultrasonic sensor circles from the `ESP32 HEAD` label, preventing visual overlapping.

### ⚙️ 3. ideaMaker 3D Assembly Screenshot
* **File**: `images/cad_assembly.jpg`
* **Features**: A premium dark-mode CAD rendering of the robot model placed on a virtual ideaMaker grid build plate, matching the black, pink, and blue color palette of the real robot.

---

## 🛠️ Instructions

### How to Recompile the LaTeX Source
To recompile the document manually using the XeLaTeX engine, run the following commands sequentially in your terminal (running twice generates the Table of Contents and internal bookmarks correctly):
```powershell
xelatex 3D_PROJECT_REPORT.tex
xelatex 3D_PROJECT_REPORT.tex
```

### How to Insert the Diagrams into the PDF (Adobe Acrobat)
To prevent Adobe Acrobat conversion errors (commonly caused by PNG transparency), standard JPEG versions of the diagrams have been provided. To place them:
1. Open the compiled [3D_PROJECT_REPORT.pdf](3D_PROJECT_REPORT.pdf) in **Adobe Acrobat**.
2. Click on the **Edit PDF** tool on the right toolbar.
3. Select **Add Image** from the top panel.
4. Browse and select the corresponding `.jpg` file from the `images/` directory:
   * **Figure 1 placeholder** $\rightarrow$ `images/quadruped_robot_blueprint.jpg`
   * **Figure 2 placeholder** $\rightarrow$ `images/esp32_pca9685_wiring_setup.jpg`
   * **Figure 4 placeholder** $\rightarrow$ `images/cad_assembly.jpg`
5. Place and align the image directly into the reserved `\vspace{10cm}` blank whitespace box above the caption label.
6. Save the document.

---

## 👥 Group E Members (CGEC ECE 2027)

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

# 3D Project Report — XeLaTeX Build Guide
> **For AI Agents:** This document is the complete authoritative specification for generating the `3D_PROJECT_REPORT.tex` for the Quadruped Robot project group at CGEC. Read every section before writing a single line of LaTeX.

---

## 1. Context & Goal

Produce a formal **project report** in the style of a WTL & MeitY industrial training submission from **Cooch Behar Government Engineering College (CGEC), Dept. of ECE, 2025**, titled:

> **"Report of Quadruped Robot Development"**

Reference document: `previous_batch_project__3d__copy.pdf` — a 3D Printing batch report from a previous year. Use it as structural and stylistic reference, **not** as content.

The report covers the group's hands-on work: ESP32 + PCA9685 + MG90S servo quadruped robot, CAD design in FreeCAD, 3D printing in PLA+/PETG, and Arduino programming.

---

## 2. Group Members

Use these **exactly** from `student_details.txt`. Do not invent or reorder.

```
1. SARUPYA GUHA         — Roll No.: 34900323046 / Reg. ID: FS08A0198
2. SOHAN GHOSH          — Roll No.: 34900323050 / Reg. ID: FS08A0202
3. SAYAN SARKAR         — Roll No.: 34900323048 / Reg. ID: FS08A0157
4. DEBANJAN CHAKRABORTY — Roll No.: 34900323015 / Reg. ID: FS08A0154
5. JOYDIP DAS           — Roll No.: 34900323020 / Reg. ID: FS08A0220
6. AUSHI SARKAR         — Roll No.: 34900323011 / Reg. ID: FS08A0288
7. ANUPAM DUTTA         — Roll No.: 34900323003 / Reg. ID: FS08A0206
8. BIKKRAM DAS          — Roll No.: 34900323012 / Reg. ID: FS08A0528
9. BIRAJ SARKAR         — Roll No.: 34900323013 / Reg. ID: FS08A0239
```

**Group label:** Group-B *(adjust if faculty specifies otherwise)*

---

## 3. Faculty & Supervisors

Mirror the reference PDF's guidance panel exactly:

```
Dr. PALASH DAS          — HOD, Dept. of ECE, CGEC
Mrs. SUSMITA BANIK BARIK — Project Engineer, WTL & MeitY Project
Mr. ARIJIT BHUNIA       — Project Co-ordinator, WTL & MeitY Project
Mr. BHASKAR MONDAL      — Faculty, WTL & MeitY Project
```

---

## 4. Compiler & Engine

```
XeLaTeX  (mandatory — do NOT use pdflatex or lualatex)
```

Run sequence:
```
xelatex 3D_PROJECT_REPORT.tex
xelatex 3D_PROJECT_REPORT.tex   ← second pass for TOC/refs
```

---

## 5. Required Packages

```latex
\usepackage[a4paper, margin=0.8in]{geometry}
\usepackage{fontspec}
\usepackage{unicode-math}
\usepackage{xcolor}
\usepackage{titlesec}
\usepackage{enumitem}
\usepackage{booktabs}
\usepackage{tabularx}
\usepackage{array}
\usepackage{graphicx}
\usepackage{hyperref}
\usepackage{fancyhdr}
\usepackage{tcolorbox}
\usepackage{listings}
\usepackage{tikz}
\usepackage{pgfplots}
\usepackage{caption}
\usepackage{setspace}
\usepackage{parskip}
```

TikZ libraries to load:
```latex
\usetikzlibrary{shapes.geometric, arrows.meta, positioning, calc, fit, backgrounds}
\pgfplotsset{compat=1.18}
\tcbuselibrary{skins, breakable}
```

---

## 6. Font Stack

```latex
\setmainfont{TeX Gyre Pagella}
\setmonofont[Scale=0.88]{TeX Gyre Cursor}
\setmathfont{Latin Modern Math}
```

> **Overleaf exception:** If generating for Overleaf `.tex` source, replace main font with `\setmainfont{Calibri}`.

---

## 7. Spacing

```latex
\setlength{\parindent}{0pt}
\setlength{\parskip}{8pt}
\onehalfspacing      % from setspace
```

---

## 8. Colour System

Define all colours in preamble. **Never use any colour not listed here.**

```latex
\definecolor{myred}{RGB}{200, 0, 0}        % Section headings
\definecolor{myblue}{RGB}{0, 50, 160}      % Subsection / question headings
\definecolor{mygreen}{RGB}{0, 140, 70}     % Definitions / key terms
\definecolor{myteal}{RGB}{0, 130, 130}     % Formulas / technical specs
\definecolor{myamber}{RGB}{180, 100, 0}    % Examples / observations
\definecolor{mypurple}{RGB}{110, 0, 160}   % Special callouts
\definecolor{codebg}{RGB}{246, 248, 250}   % Code block background
\definecolor{codecomment}{RGB}{106, 153, 85} % Code comments
\definecolor{codeborder}{RGB}{208, 215, 222} % Code block border
\definecolor{footergray}{RGB}{150, 150, 150} % Footer branding
```

---

## 9. Section Title Styling

```latex
\titleformat{\section}
  {\large\bfseries\color{myred}}
  {\thesection.}{0.5em}{}[\titlerule]

\titleformat{\subsection}
  {\normalsize\bfseries\color{myblue}}
  {\thesubsection.}{0.5em}{}

\titleformat{\subsubsection}
  {\normalsize\bfseries\color{mygreen}}
  {\thesubsubsection.}{0.5em}{}
```

---

## 10. Footer Branding (Every Page)

```latex
\pagestyle{fancy}
\fancyhf{}
\fancyfoot[C]{\small\color{footergray}\textit{Biraj's Notes — CGEC ECE 2027}}
\fancyfoot[R]{\small\thepage}
\renewcommand{\headrulewidth}{0pt}
\renewcommand{\footrulewidth}{0.4pt}
```

The footer must appear on **every page** including title, certificate, and content pages.

---

## 11. Code Block Style

```latex
\lstset{
  backgroundcolor=\color{codebg},
  basicstyle=\ttfamily\small,
  commentstyle=\color{codecomment}\itshape,
  keywordstyle=\color{myblue}\bfseries,
  stringstyle=\color{myred},
  frame=single,
  rulecolor=\color{codeborder},
  breaklines=true,
  numbers=left,
  numberstyle=\tiny\color{footergray},
  tabsize=2,
  captionpos=b
}
```

---

## 12. TikZ / Diagram Rules

- All fills: **light tints only** — `myred!10`, `myblue!10`, `mygreen!10`, `gray!15`. Never use `!50` or above.
- Text inside nodes must always remain readable on white/light backgrounds.
- Define all nodes before referencing them.
- Load required libraries (listed in Section 5).
- Wrap TikZ figures in a `tcolorbox` with `gray!15` background for visual separation.

Example safe TikZ node style:
```latex
\tikzstyle{block} = [rectangle, draw, fill=myblue!10,
    text width=4cm, text centered, rounded corners, minimum height=1cm]
\tikzstyle{arrow} = [thick, ->, >=Stealth]
```

---

## 13. Tables

```latex
\renewcommand{\arraystretch}{1.3}
```

- Use `\centering` inside table environments.
- Use `tabularx` for full-width tables with `\textwidth`.
- Column spec: `|c|c|c|` — `&` count must match spec exactly (critical rule).
- Prefer `booktabs` (`\toprule`, `\midrule`, `\bottomrule`) for clean academic look.
- No tcolorbox around tables (tcolorbox reserved for TikZ/code only).

---

## 14. Document Structure

Reproduce this structure **in order**, mirroring the reference PDF:

```
1.  Cover Page            (title, group, college, year)
2.  Certificate of Approval
3.  Title Page            (submitted by all members, supervisors, college logo, year)
4.  Acknowledgement
5.  Table of Contents     (\tableofcontents)
6.  Objective
7.  Introduction to Quadruped Robotics
8.  Journey from Concept to Assembled Robot
9.  Electronics Architecture
10. Mechanical Architecture & CAD Design
11. 3D Printing Process
12. Products Made (Printed Parts)
13. Arduino Programming
14. Results & Testing
15. Conclusion
```

---

## 15. Cover Page Specification

```latex
\begin{titlepage}
  \centering
  \vspace*{2cm}
  {\Huge\bfseries\color{myred} QUADRUPED ROBOT\par}
  \vspace{0.5cm}
  {\Huge\bfseries\color{myblue} DEVELOPMENT REPORT\par}
  \vspace{1cm}
  \vspace{10cm}   % ← reserved for cover image (robot/3D-printing themed photo)
  \vfill
  {\large Department of Electronics and Communication Engineering\par}
  {\large Cooch Behar Government Engineering College\par}
  \vspace{0.3cm}
  {\large 2025\par}
\end{titlepage}
```

---

## 16. Certificate of Approval Page

Follows the reference PDF format:

```latex
\section*{Certificate of Approval}

It is hereby approved that the project report entitled \textbf{Report of Quadruped Robot
Development} submitted by:

\begin{enumerate}[label=\arabic*.]
  \item SARUPYA GUHA (Roll No.: 34900323046 / Reg. ID: FS08A0198)
  % ... all 9 members
\end{enumerate}

As part of their certification-based industrial training in \textbf{Robotics \& Embedded Systems},
conducted under the supervision of \textbf{WTL \& MeitY, Cooch Behar Government Engineering
College}, Cooch Behar, West Bengal.

\vspace{3cm}
\begin{tabular}{p{7cm} p{7cm}}
  \rule{6cm}{0.4pt} & \rule{6cm}{0.4pt} \\
  Mrs.\ Susmita Banik Barik & Dr.\ Palash Das \\
  (Project Engineer, WTL, MeitY) & (HOD, Dept.\ of ECE, CGEC)
\end{tabular}
```

---

## 17. Title Page Specification

```latex
\begin{center}
  {\huge\textbf{REPORT FOR QUADRUPED ROBOT DEVELOPMENT}\par}
  \vspace{1cm}
  \textbf{Submitted by:}\par
  \vspace{0.5cm}
  % List all 9 members in bold, one per line, with Roll No. / Reg. ID
  {\bfseries SARUPYA GUHA} (Roll No.: 34900323046 / Reg. ID: FS08A0198)\\
  % ... all 9
  \vspace{0.5cm}
  \textbf{Group-B}\par
  \vspace{1cm}
  \textbf{Under the guidance of:}\par
  {\bfseries Dr.\ PALASH DAS} (HOD of ECE Dept., CGEC)\\
  {\bfseries Mrs.\ SUSMITA BANIK BARIK} (Project Eng., WTL \& MeitY Project)\\
  {\bfseries Mr.\ ARIJIT BHUNIA} (Project Co-ordinator, WTL \& MeitY Project)\\
  {\bfseries Mr.\ BHASKAR MONDAL} (Faculty, WTL \& MeitY Project)\par
  \vspace{1cm}
  \vspace{3cm}    % ← reserved for CGEC college logo
  \par\vspace{0.3cm}
  {\small\textit{तमसो मा ज्योतिर्गमय}}\par   % College motto in Devanagari
  \vspace{0.5cm}
  Department of Electronics and Communication Engineering\\
  Cooch Behar Government Engineering College\\
  \textbf{2025}
\end{center}
```

> Note: The Devanagari motto requires `fontspec` (XeLaTeX handles it natively).

---

## 18. Acknowledgement Page

Keep to ~3 paragraphs, styled after the reference. Mention:
- Dr. Palash Das, Mrs. Susmita Banik Barik, Mr. Arijit Bhunia, Mr. Bhaskar Mondal
- WTL & MeitY Project and the Govt. of India
- The 3D Printing lab at CGEC (Air Conditioners, Smart Board, computers, 3D printers)
- Group members' collaborative spirit

---

## 19. Table of Contents

```latex
\tableofcontents
\newpage
```

Use standard LaTeX auto-TOC. Do not manually type it.

---

## 20. Content Section Guidance

### 20.1 Objective
State the goal of the project: building an 8-DOF quadruped robot using ESP32, PCA9685, MG90S servos, FreeCAD CAD, and 3D printing. Approx. 150–200 words, two paragraphs.

### 20.2 Introduction to Quadruped Robotics
Cover: what a quadruped robot is, real-world applications (search & rescue, rough terrain, military, research), why ESP32 was chosen, and the additive manufacturing angle (3D printing for chassis). ~300 words.

### 20.3 Journey from Concept to Assembled Robot
Describe the development phases from the `.txt` file:
- Phase 1–3: Testing servos, ESP32, PCA9685 → CAD → Printing
- Phase 4–6: Calibration → Standing → Walking

Use a TikZ flowchart here (Phase 1 → Phase 2 → … → Phase 9 in a horizontal or vertical arrow chain, `myblue!10` fill).

### 20.4 Electronics Architecture
Cover: ESP32, PCA9685 I2C connection, servo channel mapping, power rail (5V external). Include the pin connection table and channel mapping table from the `.txt` file.

**Important discovery to document:**
> PCA9685 V+ rail was measured at 0.10V → servos not moving. Root cause: V+ not connected to external 5V. Resolution: provide 5V directly to V+ terminal.

### 20.5 Mechanical Architecture & CAD Design
Cover: Core Plate, Head Box, Upper Leg, Lower Leg dimensions (from `.txt`). Include FreeCAD design workflow. Use a TikZ top-view layout diagram showing FL/FR/RL/RR hip positions.

### 20.6 3D Printing Process
Cover FDM basics (brief), material choice (PLA+ and PETG), slicer settings:
- Layer Height: 0.2 mm
- Infill: 40%
- Walls: 4
- Nozzle: 0.4 mm

Reference the previous batch report for general 3D printing theory (STL file format, G-code, slicing workflow) — cite it as "Previous Batch Project Report, CGEC 2024".

### 20.7 Products Made (Printed Parts)
List each printed component with material used:

| Part | Material | Dimensions |
|---|---|---|
| Core Plate | PLA+ | 110×85×4 mm |
| Head Box | PLA+ | 85×65×45 mm |
| Upper Leg ×4 | PETG | 45×10×5 mm |
| Lower Leg ×4 | PETG | 55×10×5 mm |
| Servo Mounts | PLA+ | Custom |

Include photographs if available (`\includegraphics`). Otherwise, use the image placeholder convention from Section 21. Slots for Fig. 8, Fig. 9, and Fig. 10 must all be present.

### 20.8 Arduino Programming
Include the servo test code block (from `.txt`) with `lstlisting`. Briefly explain PCA9685 I2C scanner code and what `0x40` address confirmation means.

### 20.9 Results & Testing
Document: I2C verified at `0x40`, V+ issue diagnosed and resolved, all 9 servos individually tested, phases completed vs pending.

### 20.10 Conclusion
~250 words. Cover: what was built, what was learnt (electronics debugging, CAD, 3D printing), future roadmap (walking gait, sensor integration, autonomous navigation).

---

## 21. Image Placeholder Convention

**All image slots must use a minimum `\vspace` of `10cm`** to reserve physical space for photographs to be inserted later. Never use less than `10cm` for any image placeholder.

The standard placeholder pattern is:

```latex
\begin{figure}[h]
  \centering
  \vspace{10cm}   % ← MINIMUM 10cm — do NOT reduce this
  \caption{Descriptive caption goes here}
  \label{fig:label}
\end{figure}
```

Use this exact pattern for **every** image slot throughout the document. Do not use `\fbox` or dummy text inside the reserved space — keep it clean and empty so photos can be physically pasted or inserted later.

### Required Image Slots & Captions

The following image slots must appear in the document in the order listed. Each must follow the placeholder pattern above with its assigned caption and label:

| # | Section | Caption | Label |
|---|---|---|---|
| 1 | Cover Page | *(no caption — decorative cover image)* | — |
| 2 | Title Page | *(no caption — CGEC college logo)* | — |
| 3 | §7 Introduction | Fig. 1 — A Quadruped Robot (reference) | `fig:quadruped_ref` |
| 4 | §9 Electronics | Fig. 2 — ESP32 and PCA9685 wiring setup | `fig:wiring` |
| 5 | §9 Electronics | Fig. 3 — I2C Scanner output confirming address 0x40 | `fig:i2c_scan` |
| 6 | §10 CAD Design | Fig. 4 — FreeCAD model of Core Plate | `fig:cad_coreplate` |
| 7 | §10 CAD Design | Fig. 5 — FreeCAD model of Upper and Lower Leg | `fig:cad_legs` |
| 8 | §10 CAD Design | Fig. 6 — Full Assembly view in FreeCAD | `fig:cad_assembly` |
| 9 | §11 3D Printing | Fig. 7 — 3D Printer during a print job | `fig:printer` |
| 10 | §12 Printed Parts | Fig. 8 — 3D Printed Core Plate (Material: PLA+) | `fig:core_plate` |
| 11 | §12 Printed Parts | Fig. 9 — 3D Printed Head Box (Material: PLA+) | `fig:head_box` |
| 12 | §12 Printed Parts | Fig. 10 — 3D Printed Leg Components (Material: PETG) | `fig:legs` |
| 13 | §14 Results | Fig. 11 — All 9 Servos tested individually | `fig:servo_test` |
| 14 | §14 Results | Fig. 12 — Partially Assembled Quadruped Robot | `fig:assembly` |

### Cover & Title Page Images (Special Handling)

For the cover page and title page, the images are decorative/institutional and do not use `figure` environments. Use this pattern instead:

```latex
% Cover page image slot — decorative, no caption, no label
\vspace{10cm}   % reserved for robot/3D-printing themed cover photo

% Title page logo slot — CGEC logo
\vspace{3cm}    % reserved for CGEC logo (smaller, institutional)
```

### Replacing a Placeholder with a Real Image

When the photograph is available, replace the placeholder entirely with:

```latex
\begin{figure}[h]
  \centering
  \includegraphics[width=0.75\textwidth]{filename_without_extension}
  \caption{Caption text as listed in the table above}
  \label{fig:corresponding_label}
\end{figure}
```

Supported formats: `.jpg`, `.png`, `.pdf`. Place image files in the same directory as the `.tex` file.

> **AI Agent instruction:** Never skip an image slot. Every slot in the table above must appear in the compiled `.tex`, either as a placeholder (`\vspace{10cm}` + `\caption`) or as a real `\includegraphics`. Missing slots will break the figure numbering sequence.

---

## 22. Math Notation

- Inline math: `$...$`
- Display/block math: `$$...$$`
- Never use `\[...\]` or `\begin{equation}` unless specifically needed for numbering.

---

## 23. Listings (Code Blocks)

Use the `lstlisting` environment with language specified:

```latex
\begin{lstlisting}[language=C++, caption={Servo Test Code (ESP32)}, label={lst:servotest}]
#include <ESP32Servo.h>

Servo myServo;

void setup() {
  myServo.attach(18);
}

void loop() {
  myServo.write(0);
  delay(1000);
  myServo.write(90);
  delay(1000);
  myServo.write(180);
  delay(1000);
}
\end{lstlisting}
```

---

## 24. Hyperref Setup

```latex
\hypersetup{
  colorlinks=true,
  linkcolor=myblue,
  urlcolor=myteal,
  citecolor=mygreen,
  pdftitle={Report of Quadruped Robot Development},
  pdfauthor={Group-B, CGEC ECE 2027},
  pdfsubject={WTL MeitY Industrial Training Report}
}
```

---

## 25. Output Files

Always deliver **both**:

| File | Description |
|---|---|
| `3D_PROJECT_REPORT.tex` | Full XeLaTeX source |
| `3D_PROJECT_REPORT.pdf` | Compiled output (2 XeLaTeX passes) |

Compiled PDF must be verified: TOC populated, footer on every page, no `??` reference placeholders.

---

## 26. Critical Rules Checklist

Before finalising, verify every point:

- [ ] All 14 image slots present (placeholder or real image — none skipped)
- [ ] Every image placeholder uses `\vspace{10cm}` minimum (not less)
- [ ] Every image slot (except cover/logo) has a `\caption{}` and `\label{}`
- [ ] Figure numbering is sequential — no gaps in the Fig. 1–14 sequence
- [ ] Engine is XeLaTeX, not pdflatex
- [ ] `\usepackage[a4paper, margin=0.8in]{geometry}` present
- [ ] Fonts: TeX Gyre Pagella (main), TeX Gyre Cursor (mono), Latin Modern Math
- [ ] `\setlength{\parindent}{0pt}` and `\setlength{\parskip}{8pt}` both set
- [ ] Table `&` count matches column spec on every single row
- [ ] TikZ fills are `!10` tints only — no `!50` or darker
- [ ] Text inside TikZ nodes is always readable
- [ ] TikZ libraries loaded before use
- [ ] All 9 student names with correct Roll No. and Reg. ID
- [ ] Footer "Biraj's Notes — CGEC ECE 2027" on every page
- [ ] `\hypersetup` configured with correct metadata
- [ ] Two XeLaTeX compile passes done (TOC must resolve)
- [ ] No `??` placeholders in final PDF

---

## 27. File & Asset Checklist

The `.tex` file compiles cleanly **without any image files** — all slots are `\vspace` placeholders by default. When photographs are ready, place them in the same directory as the `.tex` and replace the corresponding placeholder with `\includegraphics`.

| Filename | What it is | Replaces |
|---|---|---|
| `cgec_logo.png` | CGEC college logo (circular, official) | `\vspace{3cm}` on title page |
| `cover_image.jpg` | Robot/robotics-themed cover photo | `\vspace{10cm}` on cover page |
| `quadruped_ref.jpg` | Reference quadruped robot photo | Fig. 1 placeholder |
| `wiring.jpg` | ESP32 + PCA9685 wiring setup photo | Fig. 2 placeholder |
| `i2c_scan.jpg` | Serial monitor screenshot of I2C scan | Fig. 3 placeholder |
| `cad_coreplate.jpg` | FreeCAD Core Plate screenshot | Fig. 4 placeholder |
| `cad_legs.jpg` | FreeCAD Leg models screenshot | Fig. 5 placeholder |
| `cad_assembly.jpg` | FreeCAD full assembly screenshot | Fig. 6 placeholder |
| `printer.jpg` | 3D printer during a print job | Fig. 7 placeholder |
| `core_plate.jpg` | Photo of printed Core Plate (PLA+) | Fig. 8 placeholder |
| `head_box.jpg` | Photo of printed Head Box (PLA+) | Fig. 9 placeholder |
| `legs.jpg` | Photo of printed leg components (PETG) | Fig. 10 placeholder |
| `servo_test.jpg` | Photo of servo testing setup | Fig. 11 placeholder |
| `assembly.jpg` | Photo of partially assembled robot | Fig. 12 placeholder |

---

*Build guide authored for CGEC ECE Group-B Quadruped Robot Project, 2025.*
*Style conventions follow Biraj Sarkar's established XeLaTeX workflow.*

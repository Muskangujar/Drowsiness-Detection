## Drowsiness Detection System

This project implements a Drowsiness Detection System for drivers using computer vision and embedded electronics. It monitors eye activity through a webcam and alerts the driver via buzzer and LCD if signs of drowsiness are detected. The system uses Python (OpenCV + dlib) for image processing, an Arduino for alerting, and Proteus for circuit simulation. It was also physically implemented on PCB.

---

### Table of Contents

- [Overview](#overview)
- [How It Works](#how-it-works)
- [Hardware Required](#hardware-required)
- [Software Used](#software-used)
- [Folder Structure](#folder-structure)
- [Setup Instructions](#setup-instructions)
- [Circuit Connections](#circuit-connections)
- [Simulation (Proteus)](#simulation-proteus)
- [Code Breakdown](#code-breakdown)
- [Photos](#photos)
- [License](#license)

---

### Overview

- **Python** captures webcam input and uses dlib to detect eyes.
- It calculates Eye Aspect Ratio (EAR) to determine if the driver is sleeping.
- Based on the status, it sends `'a'` (sleepy) or `'b'` (awake) to Arduino via Serial.
- **Arduino** receives the data and:
  - Displays status on LCD
  - Triggers a buzzer and LED for alert
- The full setup is simulated in Proteus and implemented physically using PCB.

---

### How It Works

1. Laptop webcam continuously monitors the user’s face.
2. Eye landmarks are detected and Eye Aspect Ratio (EAR) is calculated.
3. If EAR drops below threshold for a defined period:
   - Python sends `'a'` to Arduino.
   - Arduino activates buzzer, LED and LCD alert.
4. If user is awake:
   - `'b'` is sent to Arduino to show “Safe journey”.

---

### Hardware Required

| Component         | Quantity                   |
| ----------------- | -------------------------- |
| Arduino Uno       | 1                          |
| 16x2 LCD          | 1                          |
| Buzzer            | 1                          |
| Red LED           | 1                          |
| NPN Transistor    | 1                          |
| Resistors         | Few                        |
| PCB or Breadboard | 1                          |
| Jumper Wires      | As needed                  |
| 5V/9V Transformer | 1 (if powering externally) |

---

### Software Used

- Python 3.x
- OpenCV
- dlib
- imutils
- Arduino IDE
- Proteus 8 (for simulation)
- Git LFS (for large `.dat` file)

---

### Folder Structure

```
Drowsiness-Detection/
├── arduino/
│   └── drowsiness_alert.ino
├── python/
│   └── drowsiness_detection.py
├── model/
│   └── shape_predictor_68_face_landmarks.dat
├── images/
│   ├── proteus_schematic.jpg
│   └── pcb_hardware.jpg
├── README.md
└── requirements.txt
```

---

### Setup Instructions

#### Python

1. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

2. Clone and set up Git LFS for the `.dat` file:

   GitHub uses Git LFS (Large File Storage) to store the large `.dat` model file used by dlib.

   Run the following after cloning the repo:

   ```bash
   git lfs install
   git lfs pull
   ```

   This will download the required model:

   ```
   model/shape_predictor_68_face_landmarks.dat
   ```

3. Run the Python script:

   ```bash
   python python/drowsiness_detection.py
   ```

#### Arduino

1. Open `drowsiness_alert.ino` in Arduino IDE.
2. Connect Arduino via USB.
3. Upload the sketch.
4. Keep the Serial Monitor closed to avoid conflict with Python serial communication.

---

### Circuit Connections

| Arduino Pin | Connects To                    | Description                      |
| ----------- | ------------------------------ | -------------------------------- |
| D2–D7       | 16x2 LCD                       | Used to display status messages  |
| D8          | Buzzer (via transistor)        | For audible alert                |
| D9          | Red LED                        | For visual alert                 |
| GND         | Common ground                  | Shared with all components       |
| 5V          | Power supply to LCD and buzzer | Power from Arduino               |
| USB         | Laptop                         | For power + serial communication |

- **Transistor** is used between buzzer and Arduino for current control.
- **Resistors** are used for base control of transistor and current limiting for LED.

---

### Simulation (Proteus)

We used **Proteus 8** to simulate the complete circuit.

- LCD shows messages from Arduino
- Buzzer and LED trigger based on serial input
- You can simulate sending `'a'` or `'b'` using Proteus Serial Terminal

---

### Code Breakdown

#### Python Script (Detailed Explanation)

- **Captures webcam feed** using OpenCV.
- **Detects facial landmarks** using the pre-trained `.dat` model from the `dlib` library.
- **Calculates Eye Aspect Ratio (EAR)** using 6 key points around each eye.
- **Decision logic based on EAR**:
  - `EAR < 0.21` → Driver is likely **sleeping**.
  - `0.21 ≤ EAR ≤ 0.25` → Driver is **drowsy**.
  - `EAR > 0.25` → Driver is **awake**.
- Based on the EAR, the Python script sends a serial signal:
  - `'a'` → Drowsy or sleeping (alerts Arduino)
  - `'b'` → Awake (no alert needed)

#### Arduino Sketch

- Uses `LiquidCrystal` library to control LCD
- Listens to serial input from Python:
  - `'a'` = show alert, trigger buzzer + LED
  - `'b'` = show safe message, no alert
- Uses delay intervals for blink and visibility

#### Model File

- File: `shape_predictor_68_face_landmarks.dat`
- Used by dlib to locate 68 key facial points
- Pre-trained, required for face/eye tracking

---

### Photos

#### 1. PCB Hardware Setup



#### 2. Proteus Simulation



---

### License

This project is intended for academic and demonstration purposes. It was originally created by YouTube. Feel free to use, modify, and expand upon it.


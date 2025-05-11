# Raspberry Pi Line-Following Car

## Overview

The **Raspberry Pi Line-Following Car** is an educational robotics project powered by a Raspberry Pi 4 and a Pi Camera for vision-based navigation. It tracks colored lines (black, blue, green, yellow, red) and detects geometric shapes and arrows using a PID (Proportional-Integral-Derivative) control algorithm for precise navigation.

This project is inspired by the poster *"Raspberry Pi 4 Powered Automated Line Following Vehicle"* by **Danielle Lim Ern Hui** and **Hua Jia Cheng**.

---

## Hardware Requirements

- Raspberry Pi 4B  
- Pi Camera  
- L298N Motor Driver  
- DC Motors (for wheel movement)  
- Wheels  
- *(Optional)* Laptop with VNC for remote access  

## Software Requirements

- Raspbian OS (on Raspberry Pi)  
- Python 3  
- `opencv-python` library  
- `picamera2` library  

---

## Setup Instructions

### 🔌 Hardware Setup

1. Connect Raspberry Pi 4 to the L298N Motor Driver:  
   - Left Motor: Forward (GPIO 5), Backward (GPIO 6)  
   - Right Motor: Forward (GPIO 13), Backward (GPIO 19)  
   - Enable Pins: ENA (GPIO 22), ENB (GPIO 26)
2. Attach motors and wheels.
3. Connect the Pi Camera and ensure it is recognized.
4. Power components properly (5V for Raspberry Pi, ~12V for motors).

### 💻 Software Setup

```bash
sudo apt update
sudo apt install python3-pip
pip3 install opencv-python picamera2
```

1. Place `line-following-car.py` in your working directory.

---

## Usage

1. Access the Raspberry Pi (SSH, VNC, or display).
2. Navigate to the project directory:
   ```bash
   cd path/to/project
   ```
3. Run the script:
   ```bash
   python3 line-following-car.py
   ```

4. Select **two colors** to track (`black`, `blue`, `green`, `yellow`, `red`).

---

## Code Structure

### 🔧 Motor Control (GPIO + PWM)
- Left: GPIO 5 (FWD), GPIO 6 (BWD)
- Right: GPIO 13 (FWD), GPIO 19 (BWD)
- ENA: GPIO 22, ENB: GPIO 26

### 📷 Camera
- Resolution: 320x240
- Frame rate: 30 FPS using `PiCamera2`

### 🧠 Image Processing
- Converts frame to HSV color space.
- Detects lines and shapes using masks and contours.

### 📌 PID Control

```latex
$$
K_p = 70, \quad K_i = 0.01, \quad K_d = 0.2
$$
```

Correction formula:

```latex
$$
\text{corr} = K_p \cdot \text{error} + K_i \cdot \text{integral} + K_d \cdot \text{derivative}
$$
```

---

## Feature Status

| Feature               | Status      | Remarks                          |
|----------------------|-------------|----------------------------------|
| Black Line Tracking  | ✅ Working   | Stable and accurate              |
| Green/Yellow Lines   | ✅ Working   | Stable                           |
| Red Line Tracking    | ⚠️ Has errors | HSV overlap issues               |
| Blue Line Tracking   | ⚠️ Unstable  | Often confused with black        |
| Shape Detection      | ⚠️ Inaccurate| Needs contour improvements       |
| Arrow Detection      | ⚠️ Inaccurate| Misidentifies shapes             |

---

## Notes

### PID Parameters:
- `KP = 70`
- `KI = 0.01`
- `KD = 0.2`

### Challenges:
- Red/blue HSV overlaps.
- Lighting affects color detection.
- Shape/arrow detection needs refining.

### Future Improvements:
- Optimize HSV ranges.
- Improve lighting or add LED illumination.
- Enhance shape detection algorithm.
- Tune camera settings (focus, exposure).

---

## Contributors

- Danielle Lim Ern Hui  
- Hua Jia Cheng

---

## License

This project is licensed under the **MIT License** – see the [LICENSE.txt](./LICENSE.txt) file for details.

---

## References

- Project Poster: *Raspberry Pi 4 Powered Automated Line Following Vehicle*
- Project Code: `line-following-car.py`


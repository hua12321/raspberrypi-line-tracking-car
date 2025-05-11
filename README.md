Raspberry Pi Line-Following Car

Overview

The "Raspberry Pi Line-Following Car" is an educational robotics project powered by a Raspberry Pi 4 and a Pi Camera for vision-based navigation. It tracks colored lines (black, blue, green, yellow, red) and detects geometric shapes and arrows using a PID (Proportional-Integral-Derivative) control algorithm for precise navigation. This project is inspired by the poster "Raspberry Pi 4 Powered Automated Line Following Vehicle" by Danielle Lim Ern Hui and Hua Jia Cheng.

The functionality is implemented in a Python script (line-following-car.py), leveraging OpenCV for image processing by converting frames to HSV color space for robust color detection. PID control adjusts motor speeds based on the line's position, though red and blue line detection may be inaccurate due to HSV value overlaps, and shape detection needs further optimization.

Hardware Requirements





Raspberry Pi 4B



Pi Camera



L298N Motor Driver



DC Motors (for wheel movement)



Wheels



Optional: Laptop with VNC for remote access

Software Requirements





Raspbian operating system installed on Raspberry Pi



Python 3



OpenCV library



picamera2 library

Setup Instructions

Hardware Setup





Connect the Raspberry Pi 4 to the L298N Motor Driver (e.g., left motor forward on GPIO 5, backward on 6).



Attach DC motors to the driver and install wheels.



Connect the Pi Camera to the Raspberry Pi, ensuring it is powered and recognized.



Power all components with a stable 5V supply for the Raspberry Pi and appropriate voltage for motors (e.g., 12V LiPo battery).

Software Setup





Install Raspbian on the Raspberry Pi.



Install the required libraries:

sudo apt update
sudo apt install python3-pip
pip3 install opencv-python picamera2



Place the line-following-car.py file in your working directory.

Usage





Access the Raspberry Pi via SSH, VNC, or a connected display.



Open a terminal and navigate to the directory containing line-following-car.py.



Run the script:

python3 line-following-car.py



Select two colors to track (options: black, blue, green, yellow, red). The car will start tracking lines and detecting shapes, displaying results via OpenCV windows.

Code Explanation

The line-following-car.py script includes the following components:





Motor Control: Uses GPIO and PWM with pin configuration:





Left Motor: Forward (5), Backward (6)



Right Motor: Forward (13), Backward (19)



Enable Pins: ENA (22), ENB (26)



Camera Settings: Captures video at 320x240 resolution and 30 FPS using PiCamera2.



Image Processing: Converts frames to HSV color space for line detection with predefined HSV ranges.



Line Tracking: Detects the line's centroid in the frame's lower half, using PID control (KP=70, KI=0.01, KD=0.2). Motor speed limits: max=60, min=20.



Shape and Arrow Detection: Uses contour analysis to identify shapes (e.g., triangles, rectangles, circles) and arrows.



User Interaction: Allows runtime selection of two tracking colors.

PID Calculation

The calculate_pid function computes the correction value using error, prev_error, and integral. PID gains are set to KP=70, KI=0.01, KD=0.2. The formula is corr = KP * error + KI * integral + KD * derivative, where derivative = error - prev_error.

Feature Status







Feature



Status



Remarks





Black Line Tracking



Working well



Stable and accurate





Green/Yellow Line Tracking



Working well



Stable, no errors





Red Line Tracking



Has errors



HSV values change





Blue Line Tracking



Unstable



Often confused with black





Shape Detection



Inaccurate



Difficult to detect





Arrow Detection



Inaccurate



Misidentifies other objects

Notes





PID Parameters:





KP: 70



KI: 0.01



KD: 0.2



Color Detection: Depends on predefined HSV ranges, which may need adjustment for different lighting conditions.



Challenges:





Red and blue line detection issues due to HSV overlaps.



Shape and arrow detection accuracy needs improvement.



Lighting conditions can affect color detection stability.



Future Improvements:





Optimize HSV calibration for better color detection.



Improve lighting conditions or add contour verification.



Enhance shape and arrow detection algorithms.



Adjust camera focus and exposure for improved visuals.

Contributors





Danielle Lim Ern Hui



Hua Jia Cheng



References





Project Poster: "Raspberry Pi 4 Powered Automated Line Following Vehicle"



Project Code: line-following-car.py





Project Poster: "Raspberry Pi 4 Powered Automated Line Following Vehicle"



Project Code: line-following-car.py

Project Description

The "Raspberry Pi Line-Following Car" is an educational robotics project that utilizes a Raspberry Pi 4 as the central processor, paired with a Pi Camera for vision-based navigation. The car is designed to track lines of various colors (black, blue, green, yellow, red) and detect geometric shapes and arrows, employing a PID (Proportional-Integral-Derivative) control algorithm for precise tracking. Inspired by the poster "Raspberry Pi 4 Powered Automated Line Following Vehicle" by Danielle Lim Ern Hui and Hua Jia Cheng, the project outlines the visual and control processes, hardware setup, and challenges in color detection under varying lighting conditions.

Functionality is implemented through a Python script (line-following-car.py), which uses OpenCV for image processing, converting frames to HSV color space for robust color detection, and applies PID control to adjust motor speeds based on the line's position relative to the camera's center. Shape and arrow detection are additional features, though the poster notes inaccuracies, particularly with red and blue lines due to HSV value overlaps.

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





Connect the Raspberry Pi 4 to the L298N Motor Driver, ensuring correct pin configuration for motor control (e.g., left motor forward on GPIO 5, backward on 6).



Connect DC motors to the driver and attach wheels for movement.



Connect the Pi Camera to the Raspberry Pi, ensuring it is powered and recognized.



Power all components, ensuring the Raspberry Pi has a stable 5V power supply and motors have appropriate voltage (typically via a 12V LiPo battery).

Software Setup





Install Raspbian on the Raspberry Pi.



Install necessary libraries:

sudo apt update
sudo apt install python3-pip
pip3 install opencv-python picamera2



Place the line-follow- Place the line-following-car.py` file in the working directory.

Usage





Access the Raspberry Pi via SSH, VNC, or a connected display.



Open a terminal.



Navigate to the directory containing line-following-car.py.



Run the script:

python3 line-following-car.py



Select two colors to track when prompted (options include black, blue, green, yellow, red). The car will start tracking the lines and attempt to detect shapes, displaying results through OpenCV windows.

Code Explanation

The main functionality is implemented in the line-following-car.py file. Key components include:





Motor Control: Uses GPIO and PWM to control left and right motors. Pin configuration:





Left Motor: Forward (5), Backward (6)



Right Motor: Forward (13), Backward (19)



Enable Pins: ENA (22), ENB (26)



Camera Settings: Uses PiCamera2 to capture video frames at 320x240 resolution and 30 FPS.



Image Processing: Converts frames to HSV color space, applies color masks for line detection. Predefined HSV ranges for black, blue, green, yellow, and red.



Line Tracking: Detects the centroid of the line in the lower half of the frame, using PID control to adjust motor speeds. PID parameters:





KP (Proportional Gain): 70



KI (Integral Gain): 0.01



KD (Derivative Gain): 0.2



Motor Speed Limits: Maximum speed = 60, Minimum speed = 20



Shape and Arrow Detection: Uses contour analysis to detect geometric shapes (e.g., triangles, rectangles, circles) and arrows, displaying labels on the camera feed.



User Interaction: Allows users to select two tracking colors at runtime.

PID Calculation

The calculate_pid function computes the PID correction value. It takes three parameters: error, prev_error, and integral. The PID gains are set to KP=70, KI=0.01, KD=0.2. The correction value is calculated as corr = KP * error + KI * integral + KD * derivative, where derivative = error - prev_error.

Feature Status

The table below summarizes the implemented features and their current status:







Feature



Status



Remarks





Black Line Tracking



Working well



Stable and accurate





Green and Yellow Line Tracking



Working well



Stable and accurate, no errors





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



Often misidentifies other objects

Notes

PID Parameters

The PID parameters for line tracking are currently set to:





KP (Proportional Gain): 70



KI (Integral Gain): 0.01



KD (Derivative Gain): 0.2

Other Notes





Color Detection: Relies on predefined HSV ranges, which may require adjustment based on lighting conditions.



Challenges:





Red and blue line detection may encounter issues due to HSV value overlaps.



Shape and arrow detection accuracy is low and needs further optimization.



Lighting conditions can affect color detection stability.



Future Improvements:





Optimize HSV calibration to enhance color detection robustness.



Improve lighting conditions or add contour verification.



Enhance shape and arrow detection algorithms.



Adjust camera focus and exposure for better visual input.

Contributors





Danielle Lim Ern Hui



Hua Jia Cheng

License

[To be specified]

References





Project Poster: "Raspberry Pi 4 Powered Automated Line Following Vehicle"



Project Code: line-following-car.py

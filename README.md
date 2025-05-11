% Setting up the document class and necessary packages
\documentclass{article}
\usepackage{listings} % For code snippets
\usepackage{booktabs} % For professional tables
\usepackage{enumitem} % For customized lists
\usepackage[utf8]{inputenc} % For proper encoding
\usepackage[T1]{fontenc} % For font encoding
\usepackage{lmodern} % For Latin Modern font

% Configuring listings for Python code
\lstset{language=Python, basicstyle=\ttfamily\small, breaklines=true}

% Setting page margins
\usepackage[margin=1in]{geometry}

% Starting the document
\begin{document}

% Creating the title and author
\title{Raspberry Pi Line-Following Car}
\author{Danielle Lim Ern Hui, Hua Jia Cheng}
\maketitle

% Introduction section based on project description
\section{Introduction}
The ``Raspberry Pi Line-Following Car'' is an educational robotics project that utilizes a Raspberry Pi 4 as the central processor, paired with a Pi Camera for vision-based navigation. The car is designed to track lines of various colors (black, blue, green, yellow, red) and detect geometric shapes and arrows, employing a PID (Proportional-Integral-Derivative) control algorithm for precise tracking. Inspired by the poster ``Raspberry Pi 4 Powered Automated Line Following Vehicle'' by Danielle Lim Ern Hui and Hua Jia Cheng, the project outlines the visual and control processes, hardware setup, and challenges in color detection under varying lighting conditions.

Functionality is implemented through a Python script (\texttt{line-following-car.py}), which uses OpenCV for image processing, converting frames to HSV color space for robust color detection, and applies PID control to adjust motor speeds based on the line's position relative to the camera's center. Shape and arrow detection are additional features, though the poster notes inaccuracies, particularly with red and blue lines due to HSV value overlaps.

% Hardware requirements section
\section{Hardware Requirements}
\begin{itemize}
    \item Raspberry Pi 4B
    \item Pi Camera
    \item L298N Motor Driver
    \item DC Motors (for wheel movement)
    \item Wheels
    \item Optional: Laptop with VNC for remote access
\end{itemize}

% Software requirements section
\section{Software Requirements}
\begin{itemize}
    \item Raspbian operating system installed on Raspberry Pi
    \item Python 3
    \item OpenCV library
    \item picamera2 library
\end{itemize}

% Setup instructions section
\section{Setup Instructions}
\subsection{Hardware Setup}
\begin{enumerate}
    \item Connect the Raspberry Pi 4 to the L298N Motor Driver, ensuring correct pin configuration for motor control (e.g., left motor forward on GPIO 5, backward on 6).
    \item Connect DC motors to the driver and attach wheels for movement.
    \item Connect the Pi Camera to the Raspberry Pi, ensuring it is powered and recognized.
    \item Power all components, ensuring the Raspberry Pi has a stable 5V power supply and motors have appropriate voltage (typically via a 12V LiPo battery).
\end{enumerate}

\subsection{Software Setup}
\begin{enumerate}
    \item Install Raspbian on the Raspberry Pi.
    \item Install necessary libraries:
        \begin{verbatim}
sudo apt update
sudo apt install python3-pip
pip3 install opencv-python picamera2
        \end{verbatim}
    \item Place the \texttt{line-following-car.py} file in the working directory.
\end{enumerate}

% Usage instructions section
\section{Usage}
\begin{enumerate}
    \item Access the Raspberry Pi via SSH, VNC, or a connected display.
    \item Open a terminal.
    \item Navigate to the directory containing \texttt{line-following-car.py}.
    \item Run the script:
        \begin{verbatim}
python3 line-following-car.py
        \end{verbatim}
    \item Select two colors to track when prompted (options include black, blue, green, yellow, red). The car will start tracking the lines and attempt to detect shapes, displaying results through OpenCV windows.
\end{enumerate}

% Code explanation section
\section{Code Explanation}
The main functionality is implemented in the \texttt{line-following-car.py} file. Key components include:
\begin{itemize}
    \item \textbf{Motor Control}: Uses GPIO and PWM to control left and right motors. Pin configuration:
        \begin{itemize}
            \item Left Motor: Forward (5), Backward (6)
            \item Right Motor: Forward (13), Backward (19)
            \item Enable Pins: ENA (22), ENB (26)
        \end{itemize}
    \item \textbf{Camera Settings}: Uses PiCamera2 to capture video frames at 320x240 resolution and 30 FPS.
    \item \textbf{Image Processing}: Converts frames to HSV color space, applies color masks for line detection. Predefined HSV ranges for black, blue, green, yellow, and red.
    \item \textbf{Line Tracking}: Detects the centroid of the line in the lower half of the frame, using PID control to adjust motor speeds. PID parameters:
        \begin{itemize}
            \item KP (Proportional Gain): 70
            \item KI (Integral Gain): 0.01
            \item KD (Derivative Gain): 0.2
        \end{itemize}
        Motor Speed Limits: Maximum speed = 60, Minimum speed = 20
    \item \textbf{Shape and Arrow Detection}: Uses contour analysis to detect geometric shapes (e.g., triangles, rectangles, circles) and arrows, displaying labels on the camera feed.
    \item \textbf{User Interaction}: Allows users to select two tracking colors at runtime.
\end{itemize}

\subsection{PID Calculation}
The \texttt{calculate\_pid} function computes the PID correction value. It takes three parameters: \texttt{error}, \texttt{prev\_error}, and \texttt{integral}. The PID gains are set to KP=70, KI=0.01, KD=0.2. The correction value is calculated as \texttt{corr = KP * error + KI * integral + KD * derivative}, where \texttt{derivative = error - prev\_error}.

% Feature status section
\section{Feature Status}
\begin{table}[h]
    \centering
    \begin{tabular}{|l|l|l|}
        \hline
        \textbf{Feature} & \textbf{Status} & \textbf{Remarks} \\
        \hline
        Black Line Tracking & Working well & Stable and accurate \\
        Green and Yellow Line Tracking & Working well & Stable and accurate, no errors \\
        Red Line Tracking & Has errors & HSV values change \\
        Blue Line Tracking & Unstable & Often confused with black \\
        Shape Detection & Inaccurate & Difficult to detect \\
        Arrow Detection & Inaccurate & Often misidentifies other objects \\
        \hline
    \end{tabular}
    \caption{Feature Status of the Line-Following Car}
    \label{tab:feature_status}
\end{table}

% Notes section
\section{Notes}
\subsection{PID Parameters}
The PID parameters for line tracking are currently set to:
\begin{itemize}
    \item KP (Proportional Gain): 70
    \item KI (Integral Gain): 0.01
    \item KD (Derivative Gain): 0.2
\end{itemize}

\subsection{Other Notes}
\begin{itemize}
    \item \textbf{Color Detection}: Relies on predefined HSV ranges, which may require adjustment based on lighting conditions.
    \item \textbf{Challenges}:
        \begin{itemize}
            \item Red and blue line detection may encounter issues due to HSV value overlaps.
            \item Shape and arrow detection accuracy is low and needs further optimization.
            \item Lighting conditions can affect color detection stability.
        \end{itemize}
    \item \textbf{Future Improvements}:
        \begin{itemize}
            \item Optimize HSV calibration to enhance color detection robustness.
            \item Improve lighting conditions or add contour verification.
            \item Enhance shape and arrow detection algorithms.
            \item Adjust camera focus and exposure for better visual input.
        \end{itemize}
\end{itemize}

% Contributors section
\section{Contributors}
\begin{itemize}
    \item Danielle Lim Ern Hui
    \item Hua Jia Cheng
\end{itemize}

% License section
\section{License}
To be specified

% References section
\section{References}
\begin{itemize}
    \item Project Poster: ``Raspberry Pi 4 Powered Automated Line Following Vehicle''
    \item Project Code: \texttt{line-following-car.py}
\end{itemize}

% Ending the document
\end{document}


Project Poster: "Raspberry Pi 4 Powered Automated Line Following Vehicle"



Project Code: line-following-car.py

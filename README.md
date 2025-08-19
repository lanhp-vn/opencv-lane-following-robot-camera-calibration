# Vision-Lane-Robot

An autonomous lane-following robot system that uses computer vision for lane detection and dual-processor control architecture for real-time navigation.

# Demo Video
https://youtube.com/shorts/wNRHnfeyHHc?feature=share

## Overview

This project implements an autonomous robot capable of following lane markings using computer vision techniques. The system combines a Raspberry Pi 4 for vision processing with an Arduino UNO for motor control, creating a responsive lane-following vehicle.

## Hardware Architecture

### Processors
- **Raspberry Pi 4 Model B**: Handles camera input and computer vision processing
- **Arduino UNO**: Controls DC motors and receives navigation commands

### Components
- **Camera**: Logitech C270 Webcam (640x480, 30fps capability)
- **Motors**: DC motors controlled via L298N motor driver
- **Power Supply**: 5V for processors, 8V for DC motors
- **Communication**: Serial connection between Raspberry Pi and Arduino

## System Features

### Computer Vision Pipeline
1. **Camera Calibration**: Automatic calibration using chessboard patterns
2. **Lane Detection**: Multi-step image processing pipeline
   - Grayscale conversion and Gaussian blur
   - Region of Interest (ROI) selection
   - Canny edge detection
   - Hough line transform
   - Lane extrapolation and midline calculation
3. **Real-time Processing**: Achieves 25 fps on Raspberry Pi 4

### Navigation Control
- **Positioning System**: Divides frame into three control ranges for precise navigation
- **Command Interface**: Seven-level command system (a-g) for different steering corrections
- **Adaptive Speed**: Variable motor speeds based on required correction intensity

## Project Structure

```
├── project-report.ipynb          # Detailed technical report and analysis
├── robot_control/
│   ├── robot_control.py         # Python vision processing and control
│   └── robot_control.ino        # Arduino motor control firmware
├── calib_images/                # Camera calibration images
│   ├── 1.jpg
│   ├── 2.jpg
│   ├── 3.jpg
│   └── 4.jpg
├── caliResult.png              # Undistorted calibration result
├── testing_image_Q2.jpg        # Test image for lane detection
├── robot_control_flowchart.png # System architecture diagram
└── requirements.pdf            # Project requirements document
```

## Technical Implementation

### Camera Calibration
- Uses 9x6 chessboard pattern for calibration
- Computes camera matrix and distortion coefficients
- Focal length: approximately 4mm (matching C270 specifications)
- Performs lens distortion correction

### Lane Detection Algorithm
1. **Preprocessing**: Grayscale conversion, Gaussian blur, intensity thresholding
2. **Edge Detection**: Canny edge detector with dilation for line connectivity
3. **Line Detection**: Hough line transform to identify lane boundaries
4. **Lane Extrapolation**: Separates left/right lanes and extrapolates full lane lines
5. **Midline Calculation**: Computes lane center for navigation reference

### Control System
- **Range-based Navigation**: Three nested ranges around frame center
- **Proportional Control**: Seven command levels for varying steering corrections
- **Serial Communication**: 9600 baud rate between Raspberry Pi and Arduino

## Performance Metrics

- **Processing Speed**: 25 fps on Raspberry Pi 4
- **Success Rate**: 80% completion rate in test environment
- **Accuracy**: Maintains lane positioning within acceptable tolerances
- **Real-time Response**: Sub-frame latency for control commands

## Usage

### Hardware Setup
1. Connect Raspberry Pi to Arduino via USB/serial cable
2. Mount Logitech C270 webcam on robot chassis
3. Connect L298N motor driver to Arduino and DC motors
4. Ensure proper power supply connections

### Software Installation
1. Install required Python packages:
   ```bash
   pip install opencv-python numpy moviepy pyserial
   ```
2. Upload `robot_control.ino` to Arduino UNO
3. Configure serial port in Python script (`/dev/ttyUSB0` or `/dev/ttyAMA0`)

### Running the System
```bash
python robot_control/robot_control.py
```

## Control Commands

| Command | Action | Description |
|---------|---------|-------------|
| 'a' | Forward | Straight ahead |
| 'b' | Slight Left | Minor left correction |
| 'c' | Left | Moderate left turn |
| 'd' | Slight Right | Minor right correction |
| 'e' | Right | Moderate right turn |
| 'f' | Sharp Left | Strong left correction |
| 'g' | Sharp Right | Strong right correction |

## Limitations

- Requires consistent lighting conditions
- Performance degrades when robot loses sight of both lane lines
- Struggles with sharp turns at track boundaries
- Dependent on clear lane markings

## Future Improvements

- Enhanced curve detection algorithms
- Adaptive lighting compensation
- Predictive control for sharp turns
- Machine learning integration for improved robustness

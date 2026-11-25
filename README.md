# 📷 Smartphone Camera Geometric Calibration & Lens Distortion Correction using OpenCV

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![OpenCV](https://img.shields.io/badge/OpenCV-5.x-green.svg)](https://opencv.org/)
[![License](https://img.shields.io/badge/License-MIT-orange.svg)](LICENSE)

An end-to-end computer vision and photogrammetry pipeline developed in Python to calibrate consumer smartphone camera sensors, eliminate non-linear lens distortion, reconstruct 3D extrinsic camera poses, and validate sub-pixel reprojection accuracy.

---

## 🌟 Overview

Real-world optical lenses deviate from the theoretical **ideal pinhole camera model**. Compact smartphone camera modules incorporate multi-element spherical lenses that bend light non-linearly, resulting in **radial barrel distortion** where straight Cartesian lines bow outward toward the image perimeter.

This project implements **Zhang's closed-form planar calibration algorithm** combined with **Levenberg-Marquardt non-linear joint optimization** in OpenCV to:
1. Detect and refine planar chessboard corners with **sub-pixel accuracy** (`cv2.cornerSubPix`).
2. Solve the $3 \times 3$ **Camera Intrinsic Matrix ($K$)** consisting of focal lengths ($f_x, f_y$) and the principal point ($c_x, c_y$).
3. Estimate the **Brown-Conrady 5-parameter distortion coefficients** ($k_1, k_2, p_1, p_2, k_3$).
4. Mathematically **undistort** captured imagery to recover pure Euclidean geometry.
5. Triangulate **3D extrinsic camera poses** and optical rays in world coordinate space.

---

## 📐 Mathematical Formulation

### 1. Pinhole Projection Model
A 3D point in world coordinates $P_w = [X_w, Y_w, Z_w]^T$ is mapped into camera frame coordinates via rigid Euclidean transformation:
$$\begin{bmatrix} X_c \\ Y_c \\ Z_c \end{bmatrix} = R \begin{bmatrix} X_w \\ Y_w \\ Z_w \end{bmatrix} + t$$

Normalized sensor coordinates:
$$x = \frac{X_c}{Z_c}, \quad y = \frac{Y_c}{Z_c}, \quad r^2 = x^2 + y^2$$

### 2. Brown-Conrady Lens Distortion Model
Physical spherical lens curvature and lens-to-sensor decentering introduce radial and tangential displacements:
$$x_{\text{dist}} = x (1 + k_1 r^2 + k_2 r^4 + k_3 r^6) + [2 p_1 x y + p_2 (r^2 + 2 x^2)]$$
$$y_{\text{dist}} = y (1 + k_1 r^2 + k_2 r^4 + k_3 r^6) + [p_1 (r^2 + 2 y^2) + 2 p_2 x y]$$

### 3. Camera Intrinsic Matrix ($K$)
Distorted normalized rays project onto digital pixel sensor coordinates $(u, v)$:
$$\begin{bmatrix} u \\ v \\ 1 \end{bmatrix} = \begin{bmatrix} f_x & 0 & c_x \\ 0 & f_y & c_y \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} x_{\text{dist}} \\ y_{\text{dist}} \\ 1 \end{bmatrix}$$

---

## 🛠️ Project Structure

```
smartphone-camera-calibration-opencv/
├── calibration_images/          # Real multi-angle smartphone chessboard captures
├── outputs/                     # Generated calibration metrics, comparison visuals, and 3D plots
├── calibrate.py                 # Core OpenCV calibration pipeline
├── chessboard_pattern.png       # Reference calibration target
└── README.md                    # Project documentation
```

---

## 🚀 Getting Started

### 1. Prerequisites
Install required dependencies:
```bash
pip install opencv-python numpy matplotlib
```

### 2. Run Calibration
```bash
python calibrate.py
```

---

## 👨‍💻 Author
**Aman Raj**  
* GitHub: [@amanraj282007](https://github.com/amanraj282007)

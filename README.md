
# TensorFlow GPU Setup for WSL2 (Windows 11)

Automated environment setup for running **TensorFlow with GPU acceleration inside WSL2** using **Miniconda**.

This project provides:

- A **single installation script** (`script.txt`) that creates a reproducible Python environment
- GPU‑enabled **TensorFlow 2.21**
- Optional GPU libraries such as **PyCUDA** and **TensorRT**
- A **verification notebook** to confirm GPU availability

This setup is designed for **Windows 11 + WSL2 + NVIDIA GPU** workflows and works well with **VS Code Remote WSL**.

---

# Overview

The installation script performs the following actions:

1. Updates the Ubuntu system packages
2. Installs essential build tools
3. Installs **Miniconda** locally in the user directory
4. Initializes Conda for the Bash shell
5. Creates a dedicated Conda environment
6. Installs **TensorFlow 2.21 with built‑in CUDA support**
7. Installs common ML and data science libraries
8. Installs Jupyter kernel support
9. Installs optional GPU compute libraries
10. Runs a quick TensorFlow GPU detection test

Important:  
CUDA and cuDNN are **automatically handled by TensorFlow's `and-cuda` build**, so no manual CUDA installation is required.

---

# Repository Structure

```
.
├── script.txt
├── verify_tensorflow_gpu.ipynb
└── README.md
```

| File | Description |
|-----|-------------|
| `script.txt` | Automated installation script |
| `verify_tensorflow_gpu.ipynb` | Notebook for GPU verification |
| `README.md` | Documentation |

---

# Requirements

## Operating System

- Windows 11
- WSL2 enabled
- Ubuntu 20.04 / 22.04 / 24.04

## Hardware

- NVIDIA GPU compatible with CUDA
- Recent NVIDIA driver installed on Windows

Install the Windows driver from:

https://www.nvidia.com/download/index.aspx

WSL automatically exposes the GPU to Linux.

---

# Verify GPU in WSL

Inside WSL run:

```
nvidia-smi
```

You should see your GPU information.

If this command fails, fix the Windows driver before continuing.

---

# Installation

## 1. Clone the repository

```
git clone https://github.com/OmarHeshamShehab/CUDA-TensorFlow-WSL-Setup.git
cd CUDA-TensorFlow-WSL-Setup
```

## 2. Make the script executable

```
chmod +x script.txt
```

## 3. Run the installer

Run as a **normal user**:

```
./script.txt
```

Do **NOT** run with `sudo`.

The script will ask for your password only when installing system packages.

---

# What the Script Installs

## System Packages

```
build-essential
wget
curl
git
python3-dev
```

## Python Environment

A Conda environment named:

```
.tf221
```

with

```
Python 3.11
```

## Python Libraries

Core packages:

```
tensorflow[and-cuda]==2.21.*
numpy
pandas
matplotlib
seaborn
ipykernel
```

Optional GPU tools:

```
pycuda
tensorrt
```

---

# Activate the Environment

After installation:

```
conda activate .tf221
```

---

# Verify TensorFlow GPU

Inside the environment run:

```python
import tensorflow as tf

print("TensorFlow:", tf.__version__)
print("GPUs:", tf.config.list_physical_devices('GPU'))
```

If configured correctly you should see something like:

```
GPUs: [PhysicalDevice(name='/physical_device:GPU:0', device_type='GPU')]
```

---

# Using the Verification Notebook

Activate the environment:

```
conda activate .tf221
```

Run Jupyter:

```
jupyter notebook verify_tensorflow_gpu.ipynb
```

This notebook runs several TensorFlow GPU checks.

---

# Using with VS Code

1. Install the **VS Code Remote - WSL extension**
2. Open the project folder in WSL
3. Select Python interpreter:

```
~/.miniconda/envs/.tf221/bin/python
```

4. Run notebooks or Python scripts with GPU acceleration.

---

# Reinstall or Remove the Environment

Remove the environment:

```
conda remove -n .tf221 --all
```

Recreate by running the script again.

---

# Troubleshooting

## TensorFlow does not detect GPU

Check:

```
nvidia-smi
```

Then inside Python:

```
import tensorflow as tf
tf.config.list_physical_devices("GPU")
```

---

## Conda command not found

Reload the shell:

```
source ~/.bashrc
```

---

## pip install errors

Upgrade pip:

```
pip install --upgrade pip
```

---

# Notes

This setup uses **TensorFlow's integrated CUDA runtime**.

Advantages:

- No manual CUDA installation
- No cuDNN configuration
- Easier upgrades

---

# Author

Omar Hesham Shehab

GitHub:

https://github.com/OmarHeshamShehab

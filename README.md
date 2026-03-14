# 🚀 TensorFlow 2.21 GPU Setup — WSL2 + VS Code

A fully automated, plug-and-play installer that sets up TensorFlow 2.21 with full GPU support inside WSL2, accessible directly from VS Code — no manual CUDA or cuDNN configuration required.

> ✅ Tested and validated on a fresh WSL2 instance with NVIDIA RTX A5000 Laptop GPU

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Hardware & Software Requirements](#-hardware--software-requirements)
- [What Gets Installed](#-what-gets-installed)
- [Repository Structure](#-repository-structure)
- [Installation](#-installation)
- [VS Code Setup](#-vs-code-setup)
- [GPU Verification](#-gpu-verification)
- [Environment Details](#-environment-details)
- [How It Works](#-how-it-works)
- [Troubleshooting](#-troubleshooting)
- [FAQ](#-faq)

---

## 🧭 Overview

This repository provides:

- **`install_tf221_gpu.sh`** — a single bash script that installs everything from scratch
- **`verify_tensorflow_gpu.ipynb`** — a Jupyter notebook to verify your GPU setup with 8 progressive tests

The installer handles everything automatically:
- System dependencies
- Miniconda installation
- Conda environment creation
- TensorFlow 2.21 with bundled CUDA and cuDNN
- CUDA library path configuration so VS Code finds the GPU without any manual steps

---

## 💻 Hardware & Software Requirements

### Windows Side
| Requirement | Details |
|---|---|
| Operating System | Windows 10 (build 19041+) or Windows 11 |
| GPU | NVIDIA GPU with CUDA Compute Capability 6.0+ |
| NVIDIA Driver | Must be installed on **Windows** — not inside WSL2 |
| WSL2 | Enabled and up to date — run `wsl --update` in PowerShell |

### WSL2 Side
| Requirement | Details |
|---|---|
| Distro | Ubuntu 22.04 or Ubuntu 24.04 recommended |
| User | Must run as a normal user — not root |
| Internet | Required during installation |

### VS Code
| Requirement | Details |
|---|---|
| VS Code | Latest version with WSL extension |
| Extensions | Python extension + Jupyter extension |

> ⚠️ **Important:** The NVIDIA driver must be installed on Windows — NOT inside WSL2. WSL2 automatically inherits GPU access from the Windows driver.

---

## 📦 What Gets Installed

### System Level (via apt)
| Package | Purpose |
|---|---|
| `build-essential` | C/C++ compiler toolchain |
| `wget`, `curl` | File download utilities |
| `git` | Version control |
| `python3-dev` | Python development headers |

### Miniconda
| Item | Details |
|---|---|
| Location | `~/miniconda` |
| Version | Latest Miniconda3 |
| Purpose | Manages isolated Python environments |

### Inside `tf221` Conda Environment
| Package | Version | Purpose |
|---|---|---|
| Python | 3.11 | Runtime |
| TensorFlow | 2.21.x | Deep learning framework |
| CUDA | 12.5.1 | GPU compute — bundled inside pip package |
| cuDNN | 9.x | Deep learning GPU primitives — bundled inside pip package |
| NumPy | Latest | Numerical computing |
| Pandas | Latest | Data manipulation |
| Matplotlib | Latest | Data visualization |
| Seaborn | Latest | Statistical visualization |
| ipykernel | Latest | Jupyter kernel for VS Code |

> 💡 CUDA and cuDNN are installed **inside the conda environment** via `pip install tensorflow[and-cuda]`. They are fully isolated and do not affect your system.

---

## 📁 Repository Structure

```
TensorFlow-GPU-WSL2/
│
├── install_tf221_gpu.sh          # Main installer script
├── verify_tensorflow_gpu.ipynb   # GPU verification notebook
├── .gitignore                    # Excludes .vscode/ and other local files
└── README.md                     # This file
```

---

## ⚙️ Installation

### Step 1 — Verify GPU is visible in WSL2

Open your WSL2 terminal and run:

```bash
nvidia-smi
```

You should see your GPU listed. If not, make sure the NVIDIA driver is installed on Windows and WSL2 is up to date:

```powershell
# Run in Windows PowerShell
wsl --update
```

---

### Step 2 — Clone the repository

```bash
git clone https://github.com/OmarHeshamShehab/TensorFlow-GPU-WSL2.git
cd TensorFlow-GPU-WSL2
```

---

### Step 3 — Create the installer script using nano

Open a new file with nano:

```bash
nano install_tf221_gpu.sh
```

Copy the full contents of `script.txt` from the repository and paste it inside nano, then save and exit:

```
Ctrl+O → Enter to save
Ctrl+X to exit
```

---

### Step 4 — Make the script executable

```bash
chmod +x install_tf221_gpu.sh
```

---

### Step 5 — Run the installer

```bash
./install_tf221_gpu.sh
```

The script will run through 7 sections automatically:

| Section | What Happens |
|---|---|
| 1 — Safety Checks | Verifies not running as root and GPU is visible |
| 2 — System Dependencies | Updates system and installs required packages |
| 3 — Miniconda | Downloads and installs Miniconda if not present |
| 4 — Conda Environment | Creates `tf221` environment with Python 3.11 |
| 5 — Python Packages | Installs TensorFlow, CUDA, cuDNN, ML libraries, ipykernel |
| 6 — CUDA Path Fix | Configures CUDA library paths so VS Code finds the GPU automatically |

---

### Step 6 — Reopen your terminal

After the script finishes, close and reopen your WSL2 terminal so conda initializes properly:

```bash
# After reopening terminal — verify environment works
conda activate tf221
python -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"
```

Expected output:
```
[PhysicalDevice(name='/physical_device:GPU:0', device_type='GPU')]
```

---

## 🖥️ VS Code Setup

### Step 1 — Open the project in VS Code

```bash
cd TensorFlow-GPU-WSL2
code .
```

### Step 2 — Open the verification notebook

Click on `verify_tensorflow_gpu.ipynb` in the VS Code file explorer.

### Step 3 — Select the kernel

In the top right corner of the notebook, click the kernel picker and select:

```
Python (tf221)
```

> If `Python (tf221)` does not appear, reload VS Code window with `Ctrl+Shift+P` → `Reload Window`

### Step 4 — Run all cells

Click **Run All** or press `Ctrl+Shift+P` → `Jupyter: Run All Cells`

---

## ✅ GPU Verification

The verification notebook runs 8 progressive tests:

| Cell | Test | What It Proves |
|---|---|---|
| 1 | TensorFlow version + GPU detection | TF installed and GPU is visible |
| 2 | CUDA + cuDNN build info | Correct CUDA 12.5.1 and cuDNN 9 versions |
| 3 | Basic GPU matmul 2000x2000 | GPU can execute operations |
| 4 | Heavy stress matmul 10000x10000 | GPU handles large workloads |
| 5 | GPU vs CPU speed comparison | GPU is genuinely faster than CPU |
| 6 | Real neural network training 5 epochs | Full end-to-end training works on GPU |
| 7 | GPU memory usage via nvidia-smi | VRAM is allocated and accessible |
| 8 | Final summary | Clean printout of all versions and GPU info |

### Expected Final Output

```
=== Final Summary ===
TensorFlow Version : 2.21.0
GPU               : NVIDIA RTX A5000 Laptop GPU
Compute Cap       : (8, 6)
CUDA Version      : 12.5.1
cuDNN Version     : 9
CUDA Build        : True

✅ All checks complete. Your GPU setup is working correctly.
```

---

## 🔬 Environment Details

| Component | Version |
|---|---|
| TensorFlow | 2.21.0 |
| Python | 3.11.x |
| CUDA | 12.5.1 |
| cuDNN | 9.x |
| Compute Capability | 6.0+ supported (sm_60, sm_70, sm_80, sm_89, compute_90) |

### Official TensorFlow Compatibility Reference

| TensorFlow | Python | CUDA | cuDNN |
|---|---|---|---|
| 2.21.0 | 3.10 – 3.13 | 12.5 | 9.3 |
| 2.20.0 | 3.9 – 3.13 | 12.5 | 9.3 |
| 2.19.0 | 3.9 – 3.12 | 12.5 | 9.3 |

---

## ⚙️ How It Works

### Why `tensorflow[and-cuda]`?

The `pip install tensorflow[and-cuda]` package bundles CUDA and cuDNN libraries directly inside the Python package under `site-packages/nvidia/`. This means:

- No system-level CUDA toolkit needed
- No version conflicts with other projects
- Fully isolated inside the `tf221` conda environment
- Deleting the environment removes everything cleanly

### Why the CUDA Path Fix?

When `tensorflow[and-cuda]` installs, it places CUDA libraries inside:
```
~/miniconda/envs/tf221/lib/python3.11/site-packages/nvidia/
```

However, TensorFlow needs these libraries to be on `LD_LIBRARY_PATH` to find them at runtime. Without this, TensorFlow loads but cannot see the GPU. VS Code is especially affected because it launches Jupyter kernels without going through a full conda activation.

The installer automatically creates:
```
~/miniconda/envs/tf221/etc/conda/activate.d/env_vars.sh
```

This file runs every time `tf221` is activated — in the terminal and in VS Code — and sets all required library paths automatically.

---

## 🔧 Troubleshooting

### ❌ `nvidia-smi` not found in WSL2

```
nvidia-smi not found or GPU not accessible
```

**Fix:**
1. Install the latest NVIDIA driver on **Windows** (not inside WSL2)
2. Update WSL2 from PowerShell: `wsl --update`
3. Restart WSL2: `wsl --shutdown` then reopen

---

### ❌ Conda activation failed

```
Failed to activate conda environment 'tf221'
```

**Fix:**
```bash
source ~/.bashrc
conda activate tf221
```

---

### ❌ GPU not detected in VS Code but works in terminal

**Fix:** Make sure you reopened VS Code after running the installer. The `activate.d` path fix requires a fresh activation. Close VS Code completely, reopen it, and select `Python (tf221)` kernel again.

---

### ❌ Anaconda Terms of Service error

```
CondaTOSNonInteractiveError: Terms of Service have not been accepted
```

**Fix:**
```bash
conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/main
conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/r
```

---

### ❌ Kernel `Python (tf221)` not showing in VS Code

**Fix:**
```bash
conda activate tf221
python -m ipykernel install --user --name tf221 --display-name "Python (tf221)"
```

Then reload VS Code window: `Ctrl+Shift+P` → `Reload Window`

---

## ❓ FAQ

**Q: Do I need to install CUDA or cuDNN manually?**
No. They are bundled inside the pip package and installed automatically by the script.

**Q: Will this work on any NVIDIA GPU?**
Yes, as long as your GPU has Compute Capability 6.0 or higher and the NVIDIA driver is installed on Windows.

**Q: Can I use this environment for other projects?**
Yes. Just select `Python (tf221)` as the kernel in any notebook inside VS Code.

**Q: Can I rename the environment?**
Yes. Change `ENV_NAME="tf221"` at the top of the script to any name before running. Everything else updates automatically.

**Q: What if I want to remove everything?**
```bash
conda env remove -n tf221
```
This removes TensorFlow, CUDA, cuDNN and all packages in one command. Miniconda itself stays — remove `~/miniconda` to uninstall it completely.

**Q: Does this work on multiple WSL instances on the same machine?**
Yes. The script is fully portable — it uses `$HOME` and `$CONDA_PREFIX` variables internally so it adapts to any username or machine name automatically.

---

## 📄 License

MIT License — free to use, modify, and distribute.

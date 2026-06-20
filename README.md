# Deep learning development environment

[![Sync release](https://github.com/gperdrizet/deeplearning-devcontainer/actions/workflows/sync-release.yml/badge.svg)](https://github.com/gperdrizet/deeplearning-devcontainer/actions/workflows/sync-release.yml)
[![Python](https://img.shields.io/badge/Python-3.12-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.11.0-EE4C2C?logo=pytorch&logoColor=white)](https://pytorch.org/)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.17-FF6F00?logo=tensorflow&logoColor=white)](https://www.tensorflow.org/)
[![CUDA](https://img.shields.io/badge/CUDA-12.8-76B900?logo=nvidia&logoColor=white)](https://developer.nvidia.com/cuda-toolkit)
[![Docker Pulls deeplearning-nvidia](https://img.shields.io/docker/pulls/gperdrizet/deeplearning-nvidia?label=deeplearning-nvidia&logo=docker)](https://hub.docker.com/r/gperdrizet/deeplearning-nvidia)
[![Docker Pulls deeplearning-cpu](https://img.shields.io/docker/pulls/gperdrizet/deeplearning-cpu?label=deeplearning-cpu&logo=docker)](https://hub.docker.com/r/gperdrizet/deeplearning-cpu)
[![Docker Pulls deeplearning-mac](https://img.shields.io/docker/pulls/gperdrizet/deeplearning-mac?label=deeplearning-mac&logo=docker)](https://hub.docker.com/r/gperdrizet/deeplearning-mac)

A ready-to-use deep learning environment for VS Code, designed for AI/ML bootcamp students. Includes both **PyTorch** and **TensorFlow** with three configurations for wide hardware compatibility.

## Requirements

**All users**
- [Docker Desktop](https://docs.docker.com/desktop/) (Windows / Mac) or [Docker Engine](https://docs.docker.com/engine/install/) (Linux)
- [VS Code](https://code.visualstudio.com/) with the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

**NVIDIA GPU users** (also required)
- NVIDIA driver ≥570 ([download](https://www.nvidia.com/Download/index.aspx))
- [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html) *(Linux only - not needed on Windows)*

> **Mac users:** GPU acceleration (Metal/MPS) does not pass through to Docker containers. The Mac configuration uses native ARM64 CPU, no extra setup needed beyond Docker Desktop.

## Quick start

1. **Fork** this repository (click **Fork** at the top of this page)

2. **Clone** your fork:
   ```bash
   git clone https://github.com/<your-username>/deeplearning-devcontainer.git
   ```

3. **Open the folder in VS Code**, then open the Command Palette (`Ctrl+Shift+P` / `Cmd+Shift+P`) and run **Dev Containers: Open Folder in Container**

   > VS Code will ask which configuration to use, pick the one that matches your machine (see table below).

4. **Verify** your setup by running `notebooks/environment_test.ipynb`

## Which configuration should I use?

| If you have... | Choose this |
|----------------|-------------|
| NVIDIA GPU (GTX 10xx / RTX / Quadro / Tesla) | **DeepLearning NVIDIA** |
| Windows or Linux machine, no NVIDIA GPU | **DeepLearning CPU** |
| Apple Silicon Mac (M1 / M2 / M3 / M4) | **DeepLearning Mac** |

Not sure if your GPU is compatible? Check: [NVIDIA CUDA GPUs](https://developer.nvidia.com/cuda-gpus) (need compute capability ≥6.0).

## Using as a template for new projects

Fork this repo once, then use it as a GitHub template to spin up new projects instantly.

### One-time setup

1. Go to your fork on GitHub
2. Click **Settings** → scroll to **Template repository** → enable it

### Creating a new project

1. Go to your fork and click **Use this template** → **Create a new repository**
2. Name your new repo and click **Create repository**
3. Clone it and start working:
   ```bash
   git clone https://github.com/<your-username>/my-new-project.git
   ```

4. **Clean it up** - remove anything that doesn't belong to your project:
   - Update `README.md` to describe your project
   - Delete unused devcontainer configs (e.g. if you only use NVIDIA, remove `cpu/` and `mac/`)
   - Remove or replace `notebooks/environment_test.ipynb` with your own notebooks
   - Clear `logs/`, `models/`, and `data/`
   ```bash
   git add -A && git commit -m "Initial project setup" && git push
   ```

## Adding Python packages

### Temporary (lost on container rebuild)

```bash
pip install <package-name>
```

### Permanent (recommended)

1. Create a `requirements.txt` in the repository root:
   ```
   scikit-image
   plotly
   ```

2. Add a `postCreateCommand` to the relevant `.devcontainer/*/devcontainer.json`:
   ```json
   "postCreateCommand": "pip install -r requirements.txt"
   ```

3. Rebuild the container (`Ctrl+Shift+P` → **Dev Containers: Rebuild Container**)

## TensorBoard

To launch TensorBoard:

1. Open the Command Palette (`Ctrl+Shift+P` / `Cmd+Shift+P`)
2. Run **Python: Launch TensorBoard**
3. Select the `logs/` directory when prompted

TensorBoard will open in a new tab inside VS Code. Save your training logs to the `logs/` directory.

## Optuna dashboard

Right-click your Optuna database file and select **Open in Optuna Dashboard**. The dashboard is also accessible via its built-in web server on port 8080.

## Keeping your fork updated

```bash
# Add upstream once
git remote add upstream https://github.com/gperdrizet/deeplearning-devcontainer.git

# Pull in updates
git fetch upstream && git merge upstream/main
```

## What's included

### NVIDIA environment

| Category | Details |
|----------|---------|
| **Base image** | `nvcr.io/nvidia/tensorflow:25.02-tf2-py3` |
| **GPU** | CUDA 12.8, cuDNN 9.7, CuPy 13.6 |
| **ML frameworks** | PyTorch 2.11, TensorFlow 2.17, Keras 3.3 |
| **Python** | 3.12, NumPy 1.26, Pandas 2.2, Scikit-learn 1.5 |
| **Tools** | JupyterLab, TensorBoard, Optuna, Plotly, python-dotenv |

### CPU environment

| Category | Details |
|----------|---------|
| **Base image** | `python:3.12-slim` |
| **ML frameworks** | PyTorch 2.11 (CPU), TensorFlow 2.17 |
| **Python** | 3.12, NumPy 1.26, Pandas 2.2, Scikit-learn 1.5 |
| **Tools** | JupyterLab, TensorBoard, Optuna, Plotly, python-dotenv |

### Mac environment

| Category | Details |
|----------|---------|
| **Base image** | `python:3.12-slim` (linux/arm64) |
| **ML frameworks** | PyTorch 2.11 (ARM64, from PyPI), TensorFlow 2.17 |
| **Python** | 3.12, NumPy 1.26, Pandas 2.2, Scikit-learn 1.5 |
| **Tools** | JupyterLab, TensorBoard, Optuna, Plotly, python-dotenv |

## GPU compatibility (NVIDIA)

Requires compute capability ≥6.0 (Pascal / GTX 10xx or newer):

| Architecture | Example GPUs | Compute Capability |
|--------------|--------------|-------------------|
| Pascal | GTX 1050-1080, Tesla P100 | 6.0-6.1 |
| Volta | Tesla V100, Titan V | 7.0 |
| Turing | RTX 2060-2080, GTX 1660 | 7.5 |
| Ampere | RTX 3060-3090, A100 | 8.0-8.6 |
| Ada Lovelace | RTX 4060-4090 | 8.9 |
| Hopper | H100, H200 | 9.0 |
| Blackwell | RTX 5070-5090, B100, B200 | 10.0 |

## Project structure

```
deeplearning-devcontainer/
├── .devcontainer/
│   ├── nvidia/
│   │   └── devcontainer.json   # NVIDIA GPU configuration
│   ├── cpu/
│   │   └── devcontainer.json   # CPU configuration
│   └── mac/
│       └── devcontainer.json   # Mac (ARM64) configuration
├── data/                       # Store datasets here
├── logs/                       # TensorBoard logs
├── models/                     # Saved model files
├── notebooks/
│   ├── environment_test.ipynb  # Verify your setup
│   └── functions/              # Helper modules for notebooks
├── .gitignore
├── LICENSE
└── README.md
```

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Docker won't start | Enable virtualization in BIOS / enable WSL2 on Windows |
| Permission denied (Linux) | Add your user to the docker group, then log out and back in |
| GPU not detected | Update NVIDIA drivers (≥570); Linux: install [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html) |
| Container build fails | Check your internet connection |
| Module not found | Add the package to `requirements.txt` and rebuild the container |


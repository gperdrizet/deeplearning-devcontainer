# Deep learning development environment

A ready-to-use deep learning environment for VS Code, designed to give AI/ML bootcamp students a consistent development environment regardless of their hardware. Includes both **PyTorch** and **TensorFlow** frameworks, with two devcontainer configurations for wide hardware compatibility: a **GPU** version with NVIDIA CUDA support and a **CPU** version for machines without a compatible GPU.

## What's included

| Category | Versions |
|----------|----------|
| **GPU** | CUDA 12.8, cuDNN 9.7 |
| **ML** | PyTorch 2.11, TensorFlow 2.17, Keras 3.3, Scikit-learn 1.5 |
| **Python** | Python 3.12, NumPy 1.26, Pandas 2.2, Matplotlib 3.10 |
| **Tools** | JupyterLab, TensorBoard, Optuna |

The GPU configuration is based on [NVIDIA's TensorFlow 25.02 container](https://docs.nvidia.com/deeplearning/frameworks/tensorflow-release-notes/rel-25-02.html). The CPU configuration is based on [Google's official TensorFlow 2.16 image](https://hub.docker.com/r/tensorflow/tensorflow/tags?name=2.16.1).

## Project structure

```
deeplearning-devcontainer/
├── .devcontainer/
│   ├── gpu/
│   │   └── devcontainer.json   # GPU dev container configuration
│   └── cpu/
│       └── devcontainer.json   # CPU dev container configuration
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

## Requirements

- **Docker** ([Windows](https://docs.docker.com/desktop/setup/install/windows-install) | [Linux](https://docs.docker.com/desktop/setup/install/linux))
- **VS Code** with the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

### GPU configuration (additional requirements)

- **NVIDIA GPU** (Pascal or newer) with driver ≥545
- **NVIDIA Container Toolkit** (Linux): [install guide](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)
- Docker configured with GPU support

### GPU compatibility

This environment requires an NVIDIA GPU with **compute capability 6.0+** (Pascal architecture or newer):

| Architecture | Example GPUs | Compute Capability |
|--------------|--------------|-------------------|
| Pascal | GTX 1050–1080, Tesla P100 | 6.0–6.1 |
| Volta | Tesla V100, Titan V | 7.0 |
| Turing | RTX 2060–2080, GTX 1660 | 7.5 |
| Ampere | RTX 3060–3090, A100 | 8.0–8.6 |
| Ada Lovelace | RTX 4060–4090 | 8.9 |
| Hopper | H100, H200 | 9.0 |
| Blackwell | RTX 5070–5090, B100, B200 | 10.0 |

Check your GPU's compute capability: [NVIDIA CUDA GPUs](https://developer.nvidia.com/cuda-gpus)

> **Note:** This environment is configured for broad GPU compatibility, supporting Pascal and newer architectures. If you have a more recent GPU (e.g. Ada Lovelace, Hopper, or Blackwell), you may benefit from using a newer CUDA version to access the latest performance optimizations and features. Consider setting up a custom environment with an updated [NVIDIA TensorFlow container](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorflow) to take full advantage of your hardware.

## Quick start

To quickly try the container environment out on your system do the following. If you want to use it for your own project, see below.

1. **Fork** this repository (click "Fork" button above)

2. **Clone** your fork:
   ```bash
   git clone https://github.com/<your-username>/deeplearning-devcontainer.git
   ```

3. **Open VS Code**

4. **Open Folder in Container** from the VS Code command palette (`Ctrl+Shift+P`), start typing `Open Folder in`...

   > VS Code will prompt you to choose a devcontainer configuration. Select **DeepLearning GPU** if your machine has a compatible NVIDIA GPU, or **DeepLearning CPU** otherwise.

5. **Verify** by running `notebooks/environment_test.ipynb`

## Using as a template for new projects

You can use your fork as a template to quickly create new deep learning projects:

### One-time setup: Make your fork a template

1. Go to your fork on GitHub
2. Click **Settings** → scroll to **Template repository**
3. Check the box to enable it

### Creating a new project from your template

1. Go to your fork on GitHub
2. Click the green **Use this template** button → **Create a new repository**
3. Enter your new repository name and settings
4. Click **Create repository**
5. **Clone** your new repository:
   ```bash
   git clone https://github.com/<your-username>/my-new-project.git
   ```
6. **Clean up** (optional): Remove the example notebooks, then add your own code:
   ```bash
   rm -rf notebooks/*.ipynb
   git add -A && git commit -m "Initial project setup"
   git push
   ```

Now you have a fresh deep learning GPU project with the dev container configuration ready to go!

## Adding Python packages

### Using pip directly

Install packages in the container terminal:

```bash
pip install <package-name>
```

> **Note:** Packages installed this way will be lost when the container is rebuilt.

### Using requirements.txt (Recommended)

For persistent packages that survive container rebuilds:

1. **Create** a `requirements.txt` file in the repository root:
   ```
   scikit-image==0.22.0
   plotly
   ```

2. **Update** the appropriate `.devcontainer/gpu/devcontainer.json` or `.devcontainer/cpu/devcontainer.json` to install packages on container creation by adding a `postCreateCommand`:
   ```json
   "postCreateCommand": "pip install -r requirements.txt"
   ```

3. **Rebuild** the container (`F1` → "Dev Containers: Rebuild Container")

Now your packages will be automatically installed whenever the container is created.

## TensorBoard

To launch TensorBoard:

1. Open the command palette (`Ctrl+Shift+P` / `Cmd+Shift+P`)
2. Run **Python: Launch TensorBoard**
3. Select the `logs/` directory when prompted

TensorBoard will open in a new tab within VS Code. Place your training logs in the `logs/` directory.

## Optuna dashboard

Access the Optuna dashboard by right clicking on your Optuna database file and selecting 'Open in Optuna Dashboard'.

> Note: the default ports for TensorBoard and Optuna are published by the container, so you can also run them via their respective built in web servers and they will be avalible on the host's localhost.

## Keeping your fork updated

```bash
# Add upstream (once)
git remote add upstream https://github.com/gperdrizet/deeplearning-GPU.git

# Sync
git fetch upstream
git merge upstream/main
```

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Docker won't start | Enable virtualization in BIOS |
| Permission denied (Linux) | Add user to docker group, then log out/in |
| GPU not detected | Update NVIDIA drivers (≥545) |
| Container build fails | Check internet connection |


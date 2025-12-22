# Hunyuan3D-2.1 Complete Installation Guide

<div align="center">

**A comprehensive step-by-step guide for installing Hunyuan3D-2.1 with ComfyUI on Windows**

[![Windows](https://img.shields.io/badge/Platform-Windows-blue?style=flat-square)](https://www.microsoft.com/windows)
[![ComfyUI](https://img.shields.io/badge/ComfyUI-v0.3.45-green?style=flat-square)](https://github.com/comfyanonymous/ComfyUI)
[![CUDA](https://img.shields.io/badge/CUDA-12.6-orange?style=flat-square)](https://developer.nvidia.com/cuda-toolkit)

</div>

---

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#1-prerequisites)
- [Installation Steps](#installation-steps)
  - [Step 1: Clone Custom Nodes](#step-1-clone-required-custom-nodes)
  - [Step 2: Download Models](#step-2-download-models)
  - [Step 3: Install Python Dependencies](#step-3-install-python-dependencies)
  - [Step 4: Launch ComfyUI](#step-4-launch-comfyui)
- [Optional Features](#optional-features)
- [Troubleshooting](#troubleshooting)
- [Quick Reference](#quick-reference)
- [Additional Resources](#additional-resources)

---

## Overview

This guide provides a complete installation walkthrough for **Hunyuan3D-2.1** using **ComfyUI Portable v0.3.45** on Windows with NVIDIA RTX GPUs.

> **Important**: This guide is specifically validated for:
> - **Windows** operating system
> - **NVIDIA RTX GPU** (CUDA support required)
> - **ComfyUI Portable (NVIDIA) v0.3.45**

---

## 1. Prerequisites

Before starting, ensure you have the following installed:

### 1.1 ComfyUI Portable (REQUIRED)

Download the **exact build** required:

**[ComfyUI Windows Portable (NVIDIA) v0.3.45](https://github.com/comfyanonymous/ComfyUI/releases/download/v0.3.45/ComfyUI_windows_portable_nvidia.7z)**

**Extraction location example:**
```
X:\ComfyUI_windows_portable_nvidia
```

> **Critical**: Do **not** mix ComfyUI versions. All steps are validated against **v0.3.45** only.

---

### 1.2 NVIDIA CUDA Toolkit 12.6

**[Download CUDA 12.6 (Windows x64)](https://developer.nvidia.com/cuda-12-6-0-download-archive?target_os=Windows&target_arch=x86_64&target_version=11&target_type=exe_local)**

> **Note**: Reboot your system after CUDA installation is complete.

---

### 1.3 Microsoft Visual C++ Build Tools

**[Download Visual C++ Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/)**

**Required components:**
- MSVC v143
- Windows 10/11 SDK
- CMake tools

---

### 1.4 Git

**[Download Git for Windows](https://git-scm.com/downloads/win)**

---

## Installation Steps

### Step 1: Clone Required Custom Nodes

Open **Command Prompt** and navigate to:

```
ComfyUI_windows_portable\ComfyUI\custom_nodes
```

#### 1.1 Hunyuan3D-2.1 Node

```bash
git clone https://github.com/visualbruno/ComfyUI-Hunyuan3d-2-1
```

#### 1.2 Hunyuan3D Wrapper (Required Dependency)

```bash
git clone https://github.com/visualbruno/ComfyUI-Hunyuan3DWrapper
```

#### 1.3 KJNodes (Version-Specific)

> **Critical**: This workflow requires KJNodes, but newer versions are incompatible due to `comfy_api.latest` changes.

```bash
git clone https://github.com/kijai/ComfyUI-KJNodes.git
cd ComfyUI-KJNodes
git checkout 37a0973
cd ..
```

> **Important**: 
> - Do **not** update KJNodes after this step
> - Do **not** use "Update all" in ComfyUI-Manager
> - This commit (37a0973) is the last known compatible version

---

### Step 2: Download Models

#### 2.1 Diffusion Model

**Download:**
**[Hunyuan3D-2.1 Diffusion Model (model.fp16.ckpt)](https://huggingface.co/tencent/Hunyuan3D-2.1/blob/main/hunyuan3d-dit-v2-1/model.fp16.ckpt)**

**Rename to:** `hunyuan3d-dit-v2-1.ckpt`

**Place in:**
```
ComfyUI\models\diffusion_models\
```

#### 2.2 VAE Model

**Download:**
**[Hunyuan3D-2.1 VAE Model (model.fp16.ckpt)](https://huggingface.co/tencent/Hunyuan3D-2.1/blob/main/hunyuan3d-vae-v2-1/model.fp16.ckpt)**

**Rename to:** `hunyuan3d-vae-v2-1.ckpt`

**Place in:**
```
ComfyUI\models\vae\
```

---

### Step 3: Install Python Dependencies

> **Critical**: All installations **must** use ComfyUI's embedded Python.

Navigate to the ComfyUI portable root:
```
ComfyUI_windows_portable
```

#### 3.1 Custom Rasterizer

```bash
python_embeded\python.exe -m pip install ComfyUI\custom_nodes\ComfyUI-Hunyuan3DWrapper\wheels\custom_rasterizer-0.1.0+torch260.cuda126-cp312-cp312-win_amd64.whl
```

#### 3.2 Mesh Inpaint Processor

```bash
python_embeded\python.exe -m pip install ComfyUI\custom_nodes\ComfyUI-Hunyuan3d-2-1\hy3dpaint\DifferentiableRenderer\dist\mesh_inpaint_processor-0.0.0-cp312-cp312-win_amd64.whl
```

#### 3.3 Python Requirements

```bash
python_embeded\python.exe -m pip install -r ComfyUI\custom_nodes\ComfyUI-Hunyuan3d-2-1\requirements.txt
```

#### 3.4 Additional Required Packages

```bash
python_embeded\python.exe -m pip install pytorch_lightning opencv-python rembg
```

**If `rembg` installation fails:**

```bash
python_embeded\python.exe -m pip install --upgrade onnxruntime pillow numpy
python_embeded\python.exe -m pip install rembg
```

#### 3.5 Upgrading from Hunyuan3D-2.0

If you're upgrading from version 2.0:

```bash
python_embeded\python.exe -m pip uninstall -y timm
python_embeded\python.exe -m pip install --no-cache-dir "timm>=0.9.12"
```

---

### Step 4: Launch ComfyUI

From the ComfyUI portable root folder, run:

```bash
run_nvidia_gpu.bat
```

**Access ComfyUI in your browser:**
```
http://127.0.0.1:8188
```

---

## Optional Features

### Background Removal (RMBG)

Background removal is handled by **rembg** (installed in Step 3.4).

> **Note**: You do **not** need MVAdapter unless your specific workflow explicitly requires it.

**If a workflow references RMBG:**
- Ensure `rembg` imports without errors
- Restart ComfyUI if needed

---

## Troubleshooting

### Error: No module named `rembg`

**Solution:**
```bash
python_embeded\python.exe -m pip install rembg
```

If this fails, upgrade dependencies first:
```bash
python_embeded\python.exe -m pip install --upgrade onnxruntime pillow numpy
python_embeded\python.exe -m pip install rembg
```

---

### Error: No module named `comfy_api.latest`

**Cause:** KJNodes version is too new for ComfyUI core.

**Solution:**
```bash
cd ComfyUI\custom_nodes\ComfyUI-KJNodes
git checkout 37a0973
cd ..\..
```

> **Do not** update KJNodes after fixing this.

---

### Missing Nodes (INTConstant, StringConstant, SetNode, GetNode)

**Cause:** KJNodes is not loading properly.

**Solution:**
1. Check ComfyUI console for errors: `IMPORT FAILED: ComfyUI-KJNodes`
2. Verify KJNodes is pinned to commit `37a0973`
3. Restart ComfyUI
4. If issues persist, re-clone and pin KJNodes:

```bash
cd ComfyUI\custom_nodes
rmdir /s /q ComfyUI-KJNodes
git clone https://github.com/kijai/ComfyUI-KJNodes.git
cd ComfyUI-KJNodes
git checkout 37a0973
```

---

## Quick Reference

### Example Workflow Location

The example workflow file is located at:

```
ComfyUI\custom_nodes\ComfyUI-Hunyuan3d-2-1\workflow_examples\Full_Workflow.json
```

Load this file from within the ComfyUI interface.

---

### Important Reminders

- **Always use ComfyUI's embedded Python** for package installations
- **Keep ComfyUI core and custom nodes in sync** (v0.3.45)
- **Avoid auto-updating custom nodes** (especially KJNodes)
- **Do not use "Update all" in ComfyUI-Manager**

---

## Additional Resources

- [ComfyUI GitHub Repository](https://github.com/comfyanonymous/ComfyUI)
- [Hunyuan3D-2.1 on Hugging Face](https://huggingface.co/tencent/Hunyuan3D-2.1)
- [Hunyuan3D-2.1 ComfyUI Node](https://github.com/visualbruno/ComfyUI-Hunyuan3d-2-1)

---

<div align="center">

**Made for the ComfyUI community**

If this guide helped you, consider giving it a star!

</div>

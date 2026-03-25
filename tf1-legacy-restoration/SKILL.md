---
name: tf1-legacy-restoration
description: Workflow for safely containerizing and running legacy 2010s-era TensorFlow 1.x GAN models like Pix2Pix.
category: mlops
---

# Legacy TF1 Restoration Workflow

## 1. When to use this skill
Use this skill whenever asked to run, debug, or restore older machine learning projects built on TensorFlow 1.x or Python 3.6 environments.

## 2. Guardrails & Constraints
- **CRITICAL:** Never attempt to install TensorFlow 1.x on the host Raspberry Pi OS. 
- You MUST use the `terminal` tool with the Docker backend to spin up an isolated `python:3.6-slim` container.

## 3. Execution Steps
1. **Container Setup:** Initialize the Docker sandbox and use `pip` to install `tensorflow==1.15.0`.
2. **Dependency Patching:** Read the legacy repository's `requirements.txt`. If `scipy==1.1.0` or older `numpy` versions are requested, automatically patch them to the highest versions compatible with TF 1.15 to avoid compilation errors on the Pi's 64-bit ARM architecture.
3. **Execution:** Run the training or inference scripts inside the container, saving all output tensors or generated media directly to the `/workspace` mount so they persist on the host drive.
4. **Report:** Provide a concise summary of any deprecated function warnings (e.g., `tf.Session()`) that will need to be modernized to migrate the model into a standard ComfyUI custom node pipeline.

# Stable Video Infinity (SVI) on Colab

### 📖 About

This project is an experimental implementation of the **Stable Video Infinity (SVI)** workflow. Typically, generating infinite-looping videos with state-of-the-art models (like Wan 2.2 14B) requires enterprise-grade GPUs with 24GB+ VRAM.

As a student, I wanted to see if I could engineer this workflow to run on the **free tier of Google Colab (Tesla T4, 16GB)**.

### ⚙️ How It Works (The Engineering)

To fit a 14B parameter model into a 16GB GPU, this notebook uses several optimization techniques:

1. **GGUF Quantization:** The model is compressed from 16-bit to **4-bit (Q4_K_M)**, reducing VRAM usage from ~28GB to ~9GB with minimal quality loss.
2. **Split Inference (SVI):** Instead of one heavy model, we swap between two specialized checkpoints during generation:
* **Phase 1 (High Noise):** Handles motion and structure.
* **Phase 2 (Low Noise):** Handles texture and fine details.


3. **Autoregression:** The video is generated in small 4-second segments. The last frame of *Segment A* becomes the input for *Segment B*, creating an infinite seamless loop.
4. **Memory Management:** The script aggressively clears VRAM (`gc.collect`) and manages dependencies dynamically to prevent Out-Of-Memory (OOM) crashes.

### 🚀 Features

* **Infinite Loops:** Generate videos of any length (determined by the number of prompts).
* **Storytelling Mode:** Chain different prompts together (e.g., *Walking -> Running -> Flying*).
* **Optimization:** Runs entirely on free cloud hardware.

### 🛠️ How to Run

1. Open the notebook in **Google Colab**.
2. Change Runtime to **T4 GPU**.
3. **Step 1:** Select your model quality and run the setup.
4. **Step 2:** Upload a starting image.
5. **Step 3:** Enter your prompts separated by `|` and run.

### 📦 Credits & Acknowledgments

This project stands on the shoulders of giants. A special thanks to:

* **[VITA@EPFL](github.com/vita-epfl/Stable-Video-Infinity)**: For the original Stable Video Infinity research and methodology that allows for high-quality infinite video generation..
* **[Isi-dev](https://github.com/Isi-dev/Google-Colab_Notebooks):** The primary inspiration for this project. Their work on optimizing Wan2.2 for Colab provided the foundation for the memory management techniques used here.
* **ComfyUI:** For the backend architecture.
* **City96:** For the GGUF quantization nodes.
* **Kijai:** For the SVI workflow logic and wrapper nodes.

---

*Created by WardayX for educational purposes.*

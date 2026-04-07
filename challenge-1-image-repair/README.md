# 🔧 Challenge 1: Image Repair + Style Transformation

> **ACM-W × Oh Crop! Generative AI Challenge** 

---

## 🧠 Overview

This challenge demonstrates a complete pipeline for image restoration and artistic style transfer. We work with intentionally degraded images, reconstruct them using a deep generative model, and then apply three distinct style transformations to the repaired output.

The pipeline runs in three stages:

```
Degraded Input  →  U-Net Autoencoder Reconstruction  →  Style Transfer
```

---

## 📁 Folder Structure

```
challenge-1-image-repair/
│
├── notebook.ipynb                  ← Fully executed pipeline notebook
│
├── outputs/
│   ├── degraded_input.png          ← Multi-stage degraded sample images
│   ├── repaired_output.png         ← U-Net reconstructed images
│   ├── style_watercolor.png        ← Stylized output: Soft Watercolor
│   ├── style_cyberpunk.png         ← Stylized output: Cyberpunk Neon
│   └── style_golden_hour.png       ← Stylized output: Warm Golden Hour
│
└── README.md
```

---

## 🗂️ Dataset

**STL-10** — A benchmark image recognition dataset of 32×32 RGB images across 10 object classes (animals, vehicles, objects).

| Split | Samples |
|---|---|
| Training | 5,000 |
| Test | 8,000 |

STL-10 was chosen for its variety of real-world image content (diverse textures, lighting, and subjects), making it a strong testbed for both degradation robustness and style generalization.

---

## 💥 Degradation Method

A **multi-stage degradation pipeline** was applied to simulate realistic image corruption:

| Stage | Method | Parameters | Simulates |
|---|---|---|---|
| 1 | Gaussian Blur | radius = 1.25 | Motion blur / out-of-focus lens |
| 2 | Additive Gaussian Noise | std = 0.15 | Camera sensor noise |
| 3 | Random Pixel Corruption | 5% dropout | Sparse missing / corrupted data |

Combining all three creates a realistically challenging degraded input while preserving enough underlying structure for the model to reconstruct from.

---

## 🤖 Generative Model: U-Net Autoencoder

A **U-Net style convolutional autoencoder** with skip connections was used for reconstruction — chosen over a vanilla autoencoder because skip connections preserve fine spatial details lost during encoding.

**Architecture:**

| Component | Details |
|---|---|
| Encoder | 3 conv blocks (3→64→128→128 channels) + MaxPool |
| Bottleneck | 128-dimensional latent representation |
| Decoder | 3 transposed conv blocks with skip connections |
| Output | Sigmoid activation → values constrained to [0, 1] |
| Normalization | BatchNorm2d after every conv layer |

**Training Configuration:**

| Hyperparameter | Value |
|---|---|
| Epochs | 10 |
| Batch Size | 128 |
| Optimizer | Adam (lr = 0.001, weight decay = 1e-5) |
| LR Scheduler | CosineAnnealingLR (T_max = 60, η_min = 1e-6) |
| Loss Function | Combined MSE + L1 (0.7 × MSE + 0.3 × L1) |

> The combined MSE + L1 loss was a deliberate design choice: MSE handles overall smoothness while L1 preserves sharp edges and fine detail — together they prevent the blurry reconstructions that pure MSE tends to produce.

---

## 🎨 Style Transformations

Three distinct style transfer approaches were applied to the repaired output, each implemented as a custom image processing pipeline:

### 1. 🖌️ Soft Watercolor Illustration
Simulates traditional watercolor painting through:
- **Stacked bilateral filtering** (×5 passes) — smooths color regions while preserving edges, mimicking paint diffusion on paper
- **Soft edge detection overlay** — adds ink-sketch lines on top
- **Pastel tone shift** — lifts the color palette to a painted, gentle feel

### 2. ⚡ Cyberpunk Neon
A high-contrast cinematic grade inspired by neon-lit urban aesthetics:
- **S-curve contrast** — crushes blacks and boosts highlights
- **Split toning** — cyan in shadows, magenta/pink in highlights
- **Glow pass** — adds bloom on bright regions

### 3. 🌅 Warm Golden Hour
Mimics the warm light quality of late afternoon sun:
- **Channel shift** — boosts reds (+20% + offset), lifts greens slightly, pulls blues down
- **Shadow lift** — softens crushed blacks for a cinematic lifted-black look
- **Saturation boost** — enriches colors without blowing out highlights
- **Soft vignette** — draws focus to the center of the frame

---

## 🔍 Key Design Decisions

- **U-Net over plain autoencoder** — skip connections allow the decoder to access encoder feature maps directly, recovering spatial detail that would otherwise be lost in the bottleneck
- **Combined loss function** — MSE alone produces blurry outputs; adding L1 at 30% weight sharpens fine details without introducing artifacts
- **CosineAnnealingLR scheduler** — prevents learning rate plateaus during later epochs, improving final reconstruction quality
- **Custom style transfer (no neural style transfer)** — kept style transformations lightweight and deterministic using OpenCV and PIL, making the pipeline fast and reproducible without requiring a second neural network

---

## ▶️ How to Run

1. Open `notebook.ipynb` in Google Colab or Jupyter
2. Ensure GPU runtime is enabled (Runtime → Change runtime type → GPU)
3. Run all cells top to bottom — the notebook is fully pre-executed with all outputs visible

> ⚠️ The STL-10 dataset (~2.6GB) downloads automatically on first run. All saved outputs are already in the `outputs/` folder.

---
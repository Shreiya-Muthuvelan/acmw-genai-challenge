# 🌿 Challenge 2: AI Fashion Designer — *Digital Garden*

> **ACM-W × Oh Crop! Generative AI Challenge** · 

---

## 🎯 Theme: Digital Garden

*Digital Garden* translates the lush, living world of botanical gardens into wearable silhouettes. The collection draws inspiration from cherry blossoms, fern fronds, wisteria, butterfly wings, and peony blooms — rendered as dresses and coats through the lens of a deep generative model.

**Color Palette:** `Soft Pink` · `Botanical Green` · `Lavender` · `Sky Blue` · `Vivid Magenta`

---

## 📁 Folder Structure

```
challenge-2-fashion-designer/
│
├── notebook.ipynb                  ← Fully executed DCGAN training notebook
│
├── outputs/
│   ├── outfit_1_sakura.jpeg        ← Cherry Blossom dress
│   ├── outfit_2_fern.jpeg          ← Fern Frond dress
│   ├── outfit_3_lavender.jpeg      ← Wisteria gown
│   ├── outfit_4_sky.jpeg           ← Butterfly wing dress
│   ├── outfit_5_peony.jpeg         ← Peony statement coat
|
├── mood_board.jpeg              ← Botanical style sheet
├── latent_interpolation.jpeg    ← 12-step morphing strip (Sakura → Peony)
└── loss_plot.jpeg               ← Generator & Discriminator loss curves
└── outfit_designs.jpeg
└── README.md
```

---

## 🤖 Model: Deep Convolutional GAN (DCGAN)

**Dataset:** Fashion-MNIST — Classes 3 (Dress) + 4 (Coat) · ~12,000 samples

| Hyperparameter | Value |
|---|---|
| Epochs | 200 |
| Batch Size | 128 |
| Learning Rate | 2×10⁻⁴ |
| Optimizer | Adam (β₁ = 0.5) |
| Loss Function | Binary Cross-Entropy |
| Latent Dimension | 100 |

**Generator:** 100-dim noise → 4× ConvTranspose2d blocks (512→256→128→64→1) → Tanh

**Discriminator:** 4× Conv2d blocks (1→64→128→256→512) → Sigmoid

Weight initialization: Normal(0, 0.02) for Conv layers · Normal(1, 0.02) for BatchNorm

---

## 🖼️ Generated Outfits

| Outfit | Botanical Inspiration | Style Name |
|---|---|---|
| Outfit 1 | 🌸 Cherry Blossom | Sakura |
| Outfit 2 | 🌿 Fern Fronds | Fern |
| Outfit 3 | 💜 Wisteria Clusters | Lavender |
| Outfit 4 | 🦋 Butterfly Wings | Sky |
| Outfit 5 | 🌺 Peony Bloom | Peony |

> 📌 All outfit images are in the `outfits/` folder. See the mood board for a full side-by-side view.

---

## 🧬 Latent Space Exploration

Two strategies were used to explore the generative space:

**1. Sampling** — 5 fixed noise vectors were selected and passed through the trained generator, producing 5 distinct design silhouettes, each assigned a botanical identity.

**2. Linear Interpolation** — A 12-step walk was performed between the latent vector of Outfit 1 (Sakura) and Outfit 5 (Peony), showing smooth morphing through the design space. This demonstrates that the model has learned a continuous, structured latent space rather than memorizing individual samples.

---

## 🎨 Creative Process & Post-Processing

Each generated grayscale silhouette was transformed with a hand-crafted botanical RGB color map:

| Style | Effect |
|---|---|
| Sakura (Blossom) | Warm rose-pink tones with violet undertones |
| Fern | Deep, rich botanical greens |
| Lavender | Dreamy lilac-purple wash |
| Sky | Cool butterfly-wing blue with aqua |
| Peony | Bold magenta statement coloring |

A **radial vignette** and **Gaussian glow pass** were applied to every outfit for editorial depth — giving the outputs a polished, fashion-editorial feel rather than raw model output.

---

## 🗂️ Mood Board

The mood board (`mood_board.png`) presents all 5 outfits alongside their botanical inspiration sources, color swatches, and the latent interpolation strip — functioning as a cohesive style sheet for the Digital Garden collection.

---

## ▶️ How to Run

1. Open `notebook.ipynb` in Google Colab or Jupyter
2. Run all cells top to bottom (the notebook is fully pre-executed with outputs visible)
3. Generated images will appear inline and be saved to the `outputs/` folder

> ⚠️ GPU runtime recommended for training. All outputs are already saved — re-running is optional.

---
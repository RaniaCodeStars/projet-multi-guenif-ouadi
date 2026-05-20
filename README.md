# 🎬 MPEG-4-lite Encoder (Python)

A **tiny educational video codec** implemented in Python, covering the **five fundamental stages of video compression**.

---

## 🚀 Overview

This project demonstrates a simplified MPEG-style pipeline, including:

| Stage                             | Technique                                                           |
| --------------------------------- | ------------------------------------------------------------------- |
| **1. Pre-processing**             | BGR → YCbCr (BT.601) + 4:2:0 chroma subsampling                     |
| **2. I-frames**                   | 8×8 DCT (`cv2.dct`) + JPEG quantization tables                      |
| **3. P-frames**                   | Three-step block matching on 16×16 macroblocks + DCT-coded residual |
| **4. Entropy coding**             | `struct`-packed bitstream → `bz2` (level 9)                         |
| **5. Evaluation & visualization** | PSNR, compression ratio, single matplotlib figure                   |

---

## 📂 Project Structure

```
mpeg_codec.py      # Core codec functions (encode, decode, helpers)
gen_test_frames.py # Generate a synthetic sample video
viz.py             # Pipeline visualization (single figure)
run.py             # CLI driver (encode / decode / viz / sweep)
```

---

## ⚙️ Installation

Install required dependencies:

```bash
pip install numpy opencv-python matplotlib
```

> ⚠️ Note: No `scipy` is used — DCT is implemented via `cv2.dct`.

---

## ▶️ Usage

### 1. Generate sample frames

```bash
python gen_test_frames.py -o sample_frames -n 12
```

### 2. Encode frames to bitstream

```bash
python run.py encode sample_frames -o video.bin --gop 8 --q 50
```

### 3. Decode video

```bash
python run.py decode video.bin -o decoded --ref sample_frames
```

- Reconstructs frames
- Computes **PSNR**

### 4. Visualize the pipeline

```bash
python run.py viz sample_frames video.bin -o pipeline.png
```

### 5. Run experiments (quality & GOP sweep)

```bash
python run.py sweep sample_frames -o experiments.png
```

---

## 📦 Bitstream Format (MV2)

```
magic(4) | version(1) | header | { frame_tag(1) | packed_arrays... }*
```

### 🔹 Array Packing Format

Each array is stored inline as:

```
ndim(1) | shape(4 × ndim) | dtype_tag(1) | raw_bytes
```

👉 The full bitstream is then compressed using:

- `bz2` compression
- Level: **9**

---

## 🎛️ Parameters

```python
Params(
    gop,
    quality,
    block=8,
    macroblock=16,
    search,
    subsample
)
```

---

## 📊 Features

- ✔️ Educational MPEG-like pipeline
- ✔️ DCT-based compression
- ✔️ Motion estimation (block matching)
- ✔️ Custom binary bitstream
- ✔️ Built-in evaluation (PSNR, compression ratio)
- ✔️ Visualization tools

---

## 🎯 Goal

This project is designed for **learning and experimentation**, providing a clear and minimal implementation of core video compression concepts.

---

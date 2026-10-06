# Seeing Isn't Structuring: Multimodal Failure Modes in Vision-Language Models

**SL-7** is a controlled diagnostic benchmark for studying whether Vision-Language Models (VLMs) can move beyond **seeing local visual information** to correctly **reconstructing, filtering, comparing, and composing structured information**.

The benchmark uses a Snakes & Ladders environment to isolate **seven multimodal failure modes** while using prerequisite calibration to avoid confusing basic perception errors with higher-level reasoning failures.

<p align="center">
  <img src="Dataset/QC_contact_sheet.png" width="850" alt="SL-7 benchmark overview">
</p>

## Benchmark

SL-7 contains **170 queries**: **150 scored items** and **20 calibration items**.

| ID | Failure Mode |
|---|---|
| **B1** | Direction Reversal |
| **B2** | Hidden Reconstruction |
| **B3** | Endpoint Absence |
| **B4** | Perception → Action |
| **B5** | Multi-Image Comparison |
| **B6** | Conditioned Counting |
| **B7** | Multi-Source Composition |

### Example: Visible vs. Hidden Structure

<p align="center">
  <img src="Dataset/dataset/images/B2_01_visible.png" width="45%" alt="Visible board">
  &nbsp;&nbsp;
  <img src="Dataset/dataset/images/B2_01_hidden.png" width="45%" alt="Hidden board">
</p>

The model first demonstrates that it can read the visible target correctly. The corresponding hidden probe then tests whether it can reconstruct the same answer from the board's latent structure.

## Key Findings

- **Hidden reconstruction is the strongest failure:** five calibrated models achieve **50/50** on visible controls but only **1/50** on hidden reconstruction probes.
- **Direction-reversal errors are highly systematic:** **20/21** probe errors follow the same predicted `T + 1` shortcut.
- **Qwen3-VL-4B** additionally shows confirmed failures in **endpoint absence**, **conditioned counting**, and **multi-source composition**.
- In the composition task, Qwen3-VL-4B reads the component inputs correctly in **20/20** cases but succeeds on only **1/10** composition queries.

## Evaluated Models

`Qwen3-VL-4B` · `Qwen3-VL-2B` · `Qwen2.5-VL-3B` · `Phi-3.5-Vision` · `InternVL3.5-4B` · `SmolVLM2-2.2B` · `Gemma-3-4B`

## Repository Structure

```text
Code/                       # Model-specific evaluation notebooks
Dataset/
├── dataset/                # Images, benchmark items, calibration items, board metadata
├── generator/              # Synthetic benchmark generation
├── eval/                   # Scoring scripts
├── analysis/               # Cross-model result aggregation
└── QC_contact_sheet.png    # Benchmark visual overview
```

## Running the Evaluation

Open the notebook for the desired model from `Code/`, configure the dataset path, and run the notebook with GPU acceleration. Predictions can then be scored and aggregated using the scripts in `Dataset/eval/` and `Dataset/analysis/`.

## Main Takeaway

> **Accurate local perception does not guarantee that a VLM can correctly use the latent structure behind what it sees.**

SL-7 is designed to study **how multimodal models fail**, not only how often they fail.

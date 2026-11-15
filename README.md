# Hybrid U-Net and YOLO11s-seg for Solar Filament Segmentation

This repository documents my work for the **Solar Filament Segmentation Challenge 2026**, organized by the Earth-Space AI Research (ESAIR) Lab.

The project explores a hybrid instance-segmentation pipeline for identifying solar filaments in GONG H-alpha solar observations.

## Project Overview

Solar filaments are elongated structures visible in H-alpha solar images. Their shapes, sizes, and low-contrast boundaries make automatic segmentation challenging.

The pipeline combines two segmentation approaches:

- **U-Net with ResNet34 encoder:** produces pixel-level filament segmentation.
- **YOLO11s-seg:** detects and segments individual filament instances.
- **Fusion and post-processing:** combines model predictions to produce instance masks.

The implementation is based on the publicly available project by [Amey Pawar](https://github.com/ameyypawar/filament-seg). The original project is credited as the source of the pipeline. My contribution focused on running and configuring the training and inference workflows, training model checkpoints, conducting fusion experiments and validation analysis, checking submission masks, and preparing the competition submission and report.

## Repository Structure

```text
solar-filament-segmentation-2026/
├── notebooks/       # Training and inference notebooks
├── checkpoints/     # Trained model checkpoints
├── logs/            # Training logs
├── results/         # Training manifest and experiment information
├── report/          # Technical report
├── README.md
├── requirements.txt
└── .gitignore
```

## Models

| Component | Configuration |
|---|---|
| Segmentation model | U-Net |
| Encoder | ResNet34 |
| U-Net input crops | 512 × 512 |
| Instance segmentation model | YOLO11s-seg |
| YOLO input size | 1024 × 1024 |
| Hardware used | 2 × NVIDIA Tesla T4 GPUs |

Trained checkpoints included in this repository:

- `unet_resnet34.pt`
- `detector_best.pt`

## Results

The hybrid approach was evaluated using an internal date-grouped validation procedure and the competition's public leaderboard.

| Evaluation | Result |
|---|---:|
| Cross-fitted internal validation PQ | 0.3916 |
| Reported cross-fitted PQ difference | +0.0338 |
| Public Kaggle leaderboard score | 0.35 |

The internal validation result and public leaderboard score are different evaluations and should not be compared as if they were the same test.

The validation analysis and its limitations are discussed in the technical report. In particular, the all-observation U-Net-only baseline and the exact aggregation convention for the reported cross-fitted difference could not be independently verified.

## Submission Validation

The generated submission was checked for:

- The required `filament_id` and `segmentation_rle` columns.
- 2048 × 2048 mask dimensions.
- Column-major RLE decoding.
- Valid run-length totals.
- Non-overlapping predicted instance masks.

The final hybrid submission contained 1,271 predicted masks across 176 of the 180 test images. Four test images had no predicted masks.

## Reproducibility

The notebooks, checkpoints, logs, and training manifest are provided to document the experiments.

The original pipeline repository should be consulted for the upstream implementation and its original setup instructions:

https://github.com/ameyypawar/filament-seg

The competition dataset is not included in this repository. Users must obtain it through the official Kaggle competition.

## Technical Report

The four-page technical report is available in:

`report/solar_filament_technical_report.pdf`

It describes the methodology, training configuration, validation procedure, results, limitations, and project attribution.

## Acknowledgements and Attribution

Credit for the original hybrid pipeline implementation belongs to the authors of the upstream project:

https://github.com/ameyypawar/filament-seg

This repository documents my experiments and competition work using that project. It does not claim that I independently authored the original pipeline.

## Competition

Solar Filament Segmentation Challenge 2026 — Earth-Space AI Research (ESAIR) Lab.
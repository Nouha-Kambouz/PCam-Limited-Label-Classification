# PCam Limited-Label Histopathology Classification

A research project studying histopathology image classification when only a fraction of training labels is available. It uses PatchCamelyon (PCam) and compares pretrained feature extractors with a CNN trained from scratch.

> **Disclaimer:** Educational/research prototype only. It is not clinically validated and must not be used for medical diagnosis.

## Project overview

We compare:
- ImageNet-pretrained **ResNet50** features.
- ImageNet-pretrained **ViT-B/16** features.
- **Linear** and **MLP** classification heads trained on extracted features.
- A **CNN trained from scratch** as a reference.

Feature-based experiments use 10%, 25%, 50%, and 100% of training labels, with three random seeds. The scratch CNN results currently use one seed and should be interpreted as preliminary.

## Dataset

PCam contains RGB histopathology patches of size 96 × 96 pixels.

| Split | Images | Positive images |
|---|---:|---:|
| Train | 262,144 | 131,072 |
| Validation | 32,768 | 16,369 |
| Test | 32,768 | 16,377 |

Validation data is used for model selection and calibration decisions. The test set is kept for final evaluation and error analysis, not model selection or hyperparameter tuning.

## Workflow

![Project workflow](figures/workflow.png)

1. Load and inspect PCam.
2. Create reproducible labeled subsets.
3. Extract ResNet50 and ViT-B/16 features.
4. Train linear and MLP heads.
5. Aggregate validation results across seeds.
6. Evaluate calibration with temperature scaling.
7. Analyze prediction errors and confidence.
8. Train a CNN from scratch for comparison.

## Dataset examples

![PCam examples](figures/01_samples.png)

## Dataset split statistics

![Dataset split statistics](figures/02_split_stats.png)

## Model architectures

![Architecture comparison](figures/architectures.png)

## Results

### Feature-based models: validation macro-F1 (mean ± standard deviation)

| Encoder | Head | 10% labels | 25% labels | 50% labels | 100% labels |
|---|---|---:|---:|---:|---:|
| ResNet50 | Linear | 0.8495 ± 0.0061 | 0.8525 ± 0.0049 | 0.8473 ± 0.0016 | 0.8531 ± 0.0076 |
| ResNet50 | MLP | 0.8374 ± 0.0061 | 0.8521 ± 0.0069 | 0.8504 ± 0.0050 | 0.8460 ± 0.0099 |
| ViT-B/16 | Linear | 0.8624 ± 0.0041 | 0.8512 ± 0.0041 | 0.8456 ± 0.0037 | 0.8468 ± 0.0041 |
| ViT-B/16 | MLP | 0.8561 ± 0.0063 | 0.8488 ± 0.0028 | 0.8535 ± 0.0106 | 0.8422 ± 0.0132 |

![Label budget curves](figures/03_label_budget_curves.png)

Pretrained representations achieve strong validation performance with a fraction of the available labels. Results do not increase monotonically with label fraction in every experiment; subset composition and finite-sample variation can contribute to this.

### CNN trained from scratch

| Training labels | Validation macro-F1 | Test macro-F1 |
|---|---:|---:|
| 10% | 0.8796 | 0.8630 |
| 25% | 0.8942 | 0.8773 |
| 50% | 0.9110 | 0.8853 |
| 100% | 0.9070 | 0.8867 |

These results use one seed and are not a fully matched statistical comparison with the three-seed feature experiments. Training schedules also differ.

### Calibration

Temperature scaling was fitted using validation data and reduced ECE in all selected settings.

| Encoder | Labels | Temperature | Raw ECE | Scaled ECE |
|---|---:|---:|---:|---:|
| ResNet50 Linear | 10% | 1.4737 | 0.0728 | 0.0243 |
| ViT-B/16 Linear | 10% | 1.2754 | 0.0346 | 0.0099 |
| ResNet50 Linear | 100% | 1.2818 | 0.0715 | 0.0421 |
| ViT-B/16 Linear | 100% | 1.1929 | 0.0369 | 0.0157 |

![Reliability diagrams](figures/04_reliability_diagrams.png)

### Error analysis

Seed-0 test-set diagnostics for the selected linear heads:

| Encoder | Labels | Error rate | FPR | FNR | Error-detection AUROC |
|---|---:|---:|---:|---:|---:|
| ResNet50 | 10% | 0.1903 | 0.0890 | 0.2917 | 0.7592 |
| ResNet50 | 25% | 0.1843 | 0.0875 | 0.2812 | 0.7787 |
| ResNet50 | 50% | 0.1835 | 0.0889 | 0.2783 | 0.7774 |
| ResNet50 | 100% | 0.1891 | 0.0794 | 0.2990 | 0.7652 |
| ViT-B/16 | 10% | 0.1697 | 0.1062 | 0.2333 | 0.7920 |
| ViT-B/16 | 25% | 0.1634 | 0.1016 | 0.2252 | 0.7890 |
| ViT-B/16 | 50% | 0.1652 | 0.1023 | 0.2281 | 0.7921 |
| ViT-B/16 | 100% | 0.1698 | 0.1011 | 0.2385 | 0.7883 |

![Error analysis](figures/05_error_analysis.png)

In these runs, ViT has lower overall error and false-negative rate, while ResNet50 has a lower false-positive rate. Error-detection AUROC indicates useful but imperfect separation of correct and incorrect predictions.

## Repository structure

```text
PCam-Limited-Label-Classification/
├── PCam_limited_labels_colab.ipynb
├── README.md
├── requirements.txt
├── RESULTS.md
├── PCam_Limited_Label_Study_Professional_Report.docx
├── figures/
│   ├── workflow.png
│   ├── architectures.png
│   ├── 01_samples.png
│   ├── 02_split_stats.png
│   ├── 03_label_budget_curves.png
│   ├── 04_reliability_diagrams.png
│   └── 05_error_analysis.png
└── results/
    ├── summary_mean_std.csv
    ├── calibration_table.csv
    └── error_analysis.csv
```

Large generated artifacts are intentionally excluded from GitHub: raw dataset files, extracted feature arrays, model checkpoints, and large prediction arrays. They can be regenerated by running the notebook. Update data and Google Drive paths for your own environment.

## Run the notebook

1. Open the notebook in Google Colab or Jupyter.
2. Install dependencies from `requirements.txt`.
3. Download or mount PCam in the location expected by the notebook.
4. Run notebook cells in order.
5. For full experiments, use a CUDA-enabled GPU and sufficient storage.

Experiments were run in Google Colab on an NVIDIA Tesla T4. Runtime varies with hardware, package versions, and feature-cache availability.

## Evaluation metrics

Macro-F1, balanced accuracy, precision, recall, confusion matrix, class-specific error rates, expected calibration error (ECE), reliability diagrams, error-detection AUROC, area under the risk-coverage curve (AURC), and risk at 90% coverage.

## Limitations

- The scratch CNN results currently use one random seed.
- Scratch CNN and feature-based experiments use different training schedules; comparisons should be interpreted cautiously.
- Label-subset composition can affect results, especially at low label fractions.
- PCam patch results do not establish clinical performance or generalization to other hospitals, scanners, or datasets.
- This is a research prototype, not a diagnostic tool.

## Reproducibility

The notebook contains the workflow. CSV files provide compact result summaries, and the accompanying report gives a longer explanation of methodology and findings. Before publishing, remove personal information, private Drive paths, API keys, and credentials from the notebook.

## Acknowledgements

This project uses the PatchCamelyon dataset and ImageNet-pretrained ResNet50 and ViT-B/16 representations. Consult the original dataset and model documentation when citing or reusing these resources.

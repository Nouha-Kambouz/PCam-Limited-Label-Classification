# PCam Limited-Label Study: Results

Validation runs: 52
Seeds: [0, 1, 2]
GPU: Tesla T4

## Validation performance (mean ± standard deviation)

All model comparisons and learning curves use validation data. The scratch CNN has one seed; feature-based models have three seeds.

| model | head | fraction | macro_f1 | bal_acc | auroc | ece | ts_ece | n_seeds |
|---|---|---|---|---|---|---|---|---|
| cnn_scratch | full | 0.1 | 0.8796 (n=1) | 0.8796 (n=1) | 0.9478 (n=1) | 0.0368 (n=1) | 0.0353 (n=1) | 1 |
| cnn_scratch | full | 0.25 | 0.8942 (n=1) | 0.8944 (n=1) | 0.9693 (n=1) | 0.0322 (n=1) | 0.0183 (n=1) | 1 |
| cnn_scratch | full | 0.5 | 0.9110 (n=1) | 0.9112 (n=1) | 0.9708 (n=1) | 0.0279 (n=1) | 0.0130 (n=1) | 1 |
| cnn_scratch | full | 1.0 | 0.9070 (n=1) | 0.9071 (n=1) | 0.9638 (n=1) | 0.0255 (n=1) | 0.0070 (n=1) | 1 |
| resnet50 | linear | 0.1 | 0.8495 ± 0.0061 | 0.8498 ± 0.0062 | 0.9351 ± 0.0074 | 0.0456 ± 0.0139 | 0.0436 ± 0.0031 | 3 |
| resnet50 | linear | 0.25 | 0.8525 ± 0.0049 | 0.8530 ± 0.0049 | 0.9401 ± 0.0011 | 0.0330 ± 0.0001 | 0.0256 ± 0.0052 | 3 |
| resnet50 | linear | 0.5 | 0.8473 ± 0.0016 | 0.8475 ± 0.0015 | 0.9317 ± 0.0036 | 0.0300 ± 0.0084 | 0.0185 ± 0.0033 | 3 |
| resnet50 | linear | 1.0 | 0.8531 ± 0.0076 | 0.8535 ± 0.0074 | 0.9349 ± 0.0061 | 0.0345 ± 0.0022 | 0.0136 ± 0.0032 | 3 |
| resnet50 | mlp | 0.1 | 0.8374 ± 0.0061 | 0.8381 ± 0.0066 | 0.9332 ± 0.0025 | 0.0504 ± 0.0041 | 0.0407 ± 0.0059 | 3 |
| resnet50 | mlp | 0.25 | 0.8521 ± 0.0069 | 0.8528 ± 0.0067 | 0.9411 ± 0.0022 | 0.0414 ± 0.0033 | 0.0194 ± 0.0046 | 3 |
| resnet50 | mlp | 0.5 | 0.8504 ± 0.0050 | 0.8508 ± 0.0047 | 0.9331 ± 0.0039 | 0.0456 ± 0.0114 | 0.0198 ± 0.0043 | 3 |
| resnet50 | mlp | 1.0 | 0.8460 ± 0.0099 | 0.8471 ± 0.0094 | 0.9396 ± 0.0031 | 0.0537 ± 0.0093 | 0.0146 ± 0.0062 | 3 |
| vit_b16 | linear | 0.1 | 0.8624 ± 0.0041 | 0.8626 ± 0.0040 | 0.9407 ± 0.0060 | 0.0382 ± 0.0061 | 0.0372 ± 0.0060 | 3 |
| vit_b16 | linear | 0.25 | 0.8512 ± 0.0041 | 0.8514 ± 0.0041 | 0.9335 ± 0.0058 | 0.0388 ± 0.0048 | 0.0278 ± 0.0041 | 3 |
| vit_b16 | linear | 0.5 | 0.8456 ± 0.0037 | 0.8458 ± 0.0037 | 0.9308 ± 0.0053 | 0.0318 ± 0.0044 | 0.0216 ± 0.0028 | 3 |
| vit_b16 | linear | 1.0 | 0.8468 ± 0.0041 | 0.8471 ± 0.0041 | 0.9295 ± 0.0031 | 0.0289 ± 0.0045 | 0.0148 ± 0.0012 | 3 |
| vit_b16 | mlp | 0.1 | 0.8561 ± 0.0063 | 0.8564 ± 0.0061 | 0.9451 ± 0.0031 | 0.0492 ± 0.0112 | 0.0368 ± 0.0109 | 3 |
| vit_b16 | mlp | 0.25 | 0.8488 ± 0.0028 | 0.8495 ± 0.0025 | 0.9416 ± 0.0030 | 0.0560 ± 0.0103 | 0.0246 ± 0.0023 | 3 |
| vit_b16 | mlp | 0.5 | 0.8535 ± 0.0106 | 0.8540 ± 0.0102 | 0.9375 ± 0.0057 | 0.0497 ± 0.0078 | 0.0223 ± 0.0012 | 3 |
| vit_b16 | mlp | 1.0 | 0.8422 ± 0.0132 | 0.8431 ± 0.0128 | 0.9318 ± 0.0068 | 0.0551 ± 0.0111 | 0.0208 ± 0.0021 | 3 |

## Calibration analysis (seed 0, bootstrap 95% CI)

| model | head | fraction | seed | temperature | test_ece_raw | test_ece_raw_ci_low | test_ece_raw_ci_high | test_ece_scaled | test_ece_scaled_ci_low | test_ece_scaled_ci_high |
|---|---|---|---|---|---|---|---|---|---|---|
| resnet50 | linear | 0.1 | 0 | 1.4737 | 0.0728 | 0.0691 | 0.0768 | 0.0243 | 0.0219 | 0.0292 |
| vit_b16 | linear | 0.1 | 0 | 1.2754 | 0.0346 | 0.0314 | 0.0384 | 0.0099 | 0.0085 | 0.0149 |
| resnet50 | linear | 1.0 | 0 | 1.2818 | 0.0715 | 0.068 | 0.0756 | 0.0421 | 0.0389 | 0.0465 |
| vit_b16 | linear | 1.0 | 0 | 1.1929 | 0.0369 | 0.0338 | 0.0411 | 0.0157 | 0.0132 | 0.0201 |

## Error analysis (seed 0)

| model | head | fraction | error_rate | FPR | FNR | high_conf_errors_ge90 | frac_pred_ge90 | err_detect_auroc | AURC | risk_at_90cov | risk_at_80cov |
|---|---|---|---|---|---|---|---|---|---|---|---|
| resnet50 | linear | 0.1 | 0.1903 | 0.089 | 0.2917 | 902 | 0.4407 | 0.7592 | 0.0798 | 0.1611 | 0.136 |
| resnet50 | linear | 0.25 | 0.1843 | 0.0875 | 0.2812 | 1201 | 0.5497 | 0.7787 | 0.0706 | 0.155 | 0.1258 |
| resnet50 | linear | 0.5 | 0.1835 | 0.0889 | 0.2783 | 947 | 0.4949 | 0.7774 | 0.0713 | 0.1528 | 0.1246 |
| resnet50 | linear | 1.0 | 0.1891 | 0.0794 | 0.299 | 1195 | 0.514 | 0.7652 | 0.0773 | 0.1592 | 0.1324 |
| vit_b16 | linear | 0.1 | 0.1697 | 0.1062 | 0.2333 | 589 | 0.4498 | 0.792 | 0.0623 | 0.137 | 0.1091 |
| vit_b16 | linear | 0.25 | 0.1634 | 0.1016 | 0.2252 | 740 | 0.4997 | 0.789 | 0.0598 | 0.1329 | 0.1042 |
| vit_b16 | linear | 0.5 | 0.1652 | 0.1023 | 0.2281 | 639 | 0.4746 | 0.7921 | 0.0592 | 0.1338 | 0.1062 |
| vit_b16 | linear | 1.0 | 0.1698 | 0.1011 | 0.2385 | 688 | 0.4836 | 0.7883 | 0.0616 | 0.139 | 0.1117 |

## Figures

- `figures/01_samples.png`
- `figures/02_split_stats.png`
- `figures/03_label_budget_curves.png`
- `figures/04_reliability_diagrams.png`
- `figures/05_error_analysis.png`

## Evaluation notes

- Model selection and aggregate comparisons use validation metrics.
- Test-set results are reported separately in the error-analysis outputs.
- Calibration and error-analysis diagnostics shown above use seed 0.
- These results do not establish clinical readiness.
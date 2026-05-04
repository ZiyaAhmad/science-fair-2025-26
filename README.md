# PulsePath
A deep learning system that predicts supraventricular arrhythmias (SVA) **12 seconds before onset** in patients with Wolff-Parkinson-White (WPW) Syndrome, using a hybrid GRU-Transformer chain model trained on real clinical ECG data.

## Background
WPW Syndrome affects up to 24 million people worldwide. While many remain asymptomatic, approximately half will experience dangerous arrhythmias, often with little to no warning. Existing solutions are either reactive (Apple Watch), invasive (implantable loop recorders), or prohibitively expensive (up to $40,000). PulsePath's model provides a non-invasive, predictive alternative.

## How It Works
The model uses a **chain pipeline** that runs two sequential classifiers on 12-second ECG windows:

1. **Model 1 (Normal vs. Abnormal)**: filters out normal sinus rhythm using a GRU + Self-Attention architecture
2. **Model 2 (SVA vs. Pre-SVA)**: classifies abnormal signals as either active SVA or pre-SVA, using the same architecture enhanced with Layer Normalization

This chain approach lets each model specialize in its own feature space, outperforming a single 3-class classifier.

## Results
| Metric | Value |
|---|---|
| Accuracy | 95.26% |
| F1 Macro-Average | 91.99% |
| Pre-SVA Sensitivity | 90.03% |
| False Positive Rate | 4.74% |
| AUROC | 97.32% |

## Repository Structure
| File | Description |
|---|---|
| `downloading_ecg.ipynb` | Downloads the MIT-BIH SVDB dataset from PhysioNet and segments it into labeled 12-second windows |
| `train_test_split.ipynb` | Creates and saves a stratified train/test split from the processed dataset |
| `model1_normal_vs_abnormal.ipynb` | Trains Model 1 (normal vs. abnormal) |
| `model2_sva_vs_presva.ipynb` | Trains Model 2 (SVA vs. pre-SVA) |
| `chain_model.ipynb` | Evaluates the full chain model pipeline and generates the confusion matrix |

## Running the Notebooks
These notebooks are designed to run on **Google Colaboratory** with a GPU runtime (we used an A100). To get started:

1. Run `downloading_ecg.ipynb` to generate `ecg_data.npz`
2. Run `train_test_split.ipynb` to generate `train_test_split.npz`
3. Train both models using `model1_normal_vs_abnormal.ipynb` and `model2_sva_vs_presva.ipynb`
4. Evaluate the full pipeline with `chain_model.ipynb`

## Dataset
ECG data sourced from the [MIT-BIH Supraventricular Arrhythmia Database (SVDB)](https://physionet.org/content/svdb/1.0.0/) via PhysioNet, containing 78 records from 66 patients with documented SVA episodes.

## Credits
Built by Ziya Ahmad and Netra Khot for the 2025-26 Synopsys Science Fair.

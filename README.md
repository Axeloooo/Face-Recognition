# Face Recognition

[![Release](https://github.com/Axeloooo/Face-Recognition/actions/workflows/release.yml/badge.svg)](https://github.com/Axeloooo/Face-Recognition/actions/workflows/release.yml)
[![Tag](https://img.shields.io/github/v/tag/Axeloooo/Face-Recognition?label=tag)](https://github.com/Axeloooo/Face-Recognition/tags)
[![License](https://img.shields.io/github/license/Axeloooo/Face-Recognition)](https://github.com/Axeloooo/Face-Recognition/blob/main/LICENSE)
[![Issues](https://img.shields.io/github/issues/Axeloooo/Face-Recognition)](https://github.com/Axeloooo/Face-Recognition/issues)
[![Pull Requests](https://img.shields.io/github/issues-pr/Axeloooo/Face-Recognition)](https://github.com/Axeloooo/Face-Recognition/pulls)
[![Contributors](https://img.shields.io/github/contributors/Axeloooo/Face-Recognition)](https://github.com/Axeloooo/Face-Recognition/graphs/contributors)
[![Repo Size](https://img.shields.io/github/repo-size/Axeloooo/Face-Recognition)](https://github.com/Axeloooo/Face-Recognition)

---

## Table of Contents

- [Introduction](#introduction)
- [Motivation](#motivation)
- [Dataset](#dataset)
- [Methods](#methods)
  - [Feature Extraction](#feature-extraction)
  - [Support Vector Machines](#support-vector-machines)
  - [Multi-Layer Perceptron](#multi-layer-perceptron)
- [Results](#results)
- [Conclusions](#conclusions)
- [Running the Code](#running-the-code)
- [Contributors](#contributors)
- [License](#license)

---

## Introduction

This project implements and evaluates a face recognition pipeline on the **AT&T face database** — a benchmark dataset of 400 grayscale images spanning 40 subjects. Two classical machine learning classifiers are explored: **Support Vector Machines (SVM)** and **Multi-Layer Perceptrons (MLP)**. Each classifier is paired with two feature extraction strategies — **Principal Component Analysis (PCA)** and **Local Binary Patterns (LBP)** — yielding four model combinations that are systematically benchmarked against each other.

---

## Motivation

Face recognition is a fundamental problem in computer vision with applications ranging from security systems and access control to human-computer interaction. Despite the rapid rise of deep learning approaches, classical machine learning methods remain competitive on constrained datasets, are interpretable, and require far less computational resources. This project investigates how much accuracy can be achieved on a standard benchmark using only traditional feature engineering and linear/non-linear classifiers, and what trade-offs exist between feature representation quality and model complexity.

---

## Dataset

The **AT&T face database** contains:

| Property           | Value                       |
| ------------------ | --------------------------- |
| Subjects           | 40                          |
| Images per subject | 10                          |
| Total images       | 400                         |
| Resolution         | 92 × 112 px                 |
| Bit depth          | 8-bit grayscale             |
| Train / Test split | 320 / 80 (stratified 80/20) |

Images capture variability in lighting conditions, facial expressions (open/closed eyes, smiling/not smiling), facial details (glasses/no glasses), and head pose (rotations up to ~20°).

The dataset is gitignored. Restore it from the tracked archive:

```bash
unzip data/dataset.zip -d data/dataset
```

---

## Methods

### Feature Extraction

Two feature extraction strategies are evaluated:

**Principal Component Analysis (PCA)**

- Reduces each 92×112 image (10,304 raw pixels) to 100 principal components
- Captures the directions of maximum variance across the dataset (Eigenfaces)
- Preserves global facial structure while discarding noise

**Local Binary Patterns (LBP)**

- Encodes local texture by comparing each pixel to its circular neighbourhood
- Produces a 64-dimensional histogram representing texture frequency
- Captures fine-grained structural patterns robust to monotonic illumination changes

### Support Vector Machines

SVM finds the maximum-margin hyperplane separating classes in the feature space. Four kernels are evaluated via grid search:

| Kernel     | Description                                                           |
| ---------- | --------------------------------------------------------------------- |
| RBF        | Radial Basis Function — maps features into infinite-dimensional space |
| Linear     | Linear decision boundary in feature space                             |
| Polynomial | Captures polynomial feature interactions                              |
| Sigmoid    | Approximates a neural network output layer                            |

Hyperparameters `C` and `gamma` are tuned via cross-validated grid search. The one-vs-one multi-class strategy is used for the 40-class problem.

### Multi-Layer Perceptron

MLP is a feedforward neural network trained with backpropagation. The following configuration space is explored via grid search:

| Hyperparameter      | Values            |
| ------------------- | ----------------- |
| Hidden layer sizes  | `(64,)`, `(128,)` |
| Activation function | `relu`, `tanh`    |
| Optimizer           | `adam`, `sgd`     |

The best architecture is selected based on cross-validated accuracy on the training set.

---

## Results

| Model         | Test Accuracy | ROC AUC    |
| ------------- | ------------- | ---------- |
| **MLP — PCA** | **95.00%**    | **0.9969** |
| SVM — PCA     | 93.75%        | 0.9970     |
| MLP — LBP     | 88.75%        | 0.9944     |
| SVM — LBP     | 87.25%        | 0.9906     |

PCA-based features outperform LBP-based features by approximately **6–6.5 percentage points** across both classifiers. SVM — PCA achieves the fastest training time (~35.5 s) while remaining within 1.25% of the top accuracy.

---

## Conclusions

- **PCA is the superior feature extractor** for this dataset: compressing 10,304 pixel dimensions down to 100 principal components retains more discriminative information than 64-dimensional LBP histograms.
- **MLP — PCA achieves the best overall accuracy** at 95.00%, demonstrating that even a shallow neural network can surpass SVM when given strong global features.
- **SVM — PCA offers the best accuracy-efficiency trade-off**: marginally lower accuracy than MLP — PCA but with significantly shorter training time, making it preferable in resource-constrained settings.
- **LBP features underperform** on this dataset, likely because the AT&T images are low-resolution (92×112 px) and global facial geometry carries more class-discriminative signal than local texture patterns.
- Both classifiers achieve **ROC AUC > 0.99**, confirming strong separability in the learned feature spaces regardless of the extraction method.

---

## Running the Code

### Prerequisites

- Python 3.11+
- Jupyter Notebook or JupyterLab
- Google Colab (optional, for cloud execution)

### Setup

1. Clone the repository:

```bash
git clone https://github.com/Axeloooo/Face-Recognition.git
cd Face-Recognition
```

2. Create and activate a virtual environment (only if running locally; skip if using Anaconda, Miniconda, or Google Colab):

```bash
python -m venv .venv
source .venv/bin/activate   # macOS / Linux
.venv\Scripts\activate      # Windows
```

3. Install dependencies (only if running locally; skip if using Anaconda, Miniconda, or Google Colab):

```bash
 pip install -r requirements.txt
```

4. Restore the dataset (optional):

```bash
unzip data/dataset.zip -d data/dataset
```

5. Launch the notebook :

```bash
jupyter notebook face_recognition.ipynb
```

6. Run all cells with **Kernel → Restart & Run All**.

---

## Contributors

| Name                      | UCID     | GitHub Username |
| ------------------------- | -------- | --------------- |
| Axel Omar Sanchez Peralta | 30145429 | Axeloooo        |
| Mariia Podgaietska        | 30151330 | podgaietska     |

---

## License

This project is licensed under the terms of the [Apache License 2.0](LICENSE). See the LICENSE file for details.

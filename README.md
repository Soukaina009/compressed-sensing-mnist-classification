# Compressed Sensing vs Baseline LDA for Image Classification (MNIST)

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![Topic](https://img.shields.io/badge/Focus-Compressed_Sensing-green.svg)
![Dataset](https://img.shields.io/badge/Dataset-MNIST-orange.svg)

A comparison between a classic classification approach (LDA on raw pixels) and a Compressed Sensing (CS) framework combined with LDA, tested on the MNIST digit dataset.

## Why This Project

Devices like sensors, cameras, and IoT hardware often cannot store or send full high-resolution data. They need a way to capture only the useful information, using fewer measurements. Compressed Sensing offers this: it captures a compressed version of an image directly at acquisition time, using far fewer values than the original image. This project tests whether that compressed data still classifies well, compared to using the full image.

## The Question This Project Answers

Can we cut the size of MNIST images by more than half and still classify digits with LDA almost as well as on the full image?

## Methodology

The project compares three strategies:

1. **Baseline LDA**: trains directly on the raw 784-pixel image (28x28).
2. **Compressed Sensing – Direct Classification (Approach A)**: a random Gaussian projection reduces each image from 784 to about 225 measurements ($y = C \cdot x$), a number computed automatically from the wavelet sparsity of the images. LDA then classifies directly on these compressed values, with no reconstruction step.
3. **Compressed Sensing – Reconstruction (Approach B, via OMP)**: the same compressed measurements are used, but the image is first reconstructed with Orthogonal Matching Pursuit (OMP) before LDA classifies it.

To find the best number of measurements, the project first tests three wavelet families (Haar, Daubechies 2, Symlet 2) and picks the one that packs the most energy into the fewest coefficients. This sparsity level is what makes accurate compressed measurement possible, and it directly sets how many measurements the "sensor" needs to take.

## Results

| Metric | Baseline (No CS) | CS – Direct Classification (A) | CS – Reconstruction via OMP (B) |
|---|---|---|---|
| Input dimension | 784 variables | **225 variables** | ~784 variables recovered |
| Storage required | 100% | **~28.7% (71.3% saved)** | ~35% at capture |
| Runtime | ~0.12 s | **~0.03 s** (instant) | ~15.50 s (OMP reconstruction) |
| Train accuracy | 92.73% | **89.65%** | ~85.00% |
| Test accuracy | 83.80% | **86.20%** | ~80.00% |
| Train/test gap | 8.93% | **3.45%** | ~5.00% |
| Generalization | Slight overfitting | **Excellent** | Moderate |

**Main takeaways:**

- The direct classification approach (A) removes 71.3% of the input data and still scores *higher* on the test set than the full-resolution baseline (86.20% vs. 83.80%). The wavelet compression filters out high-frequency noise, so the model overfits less and generalizes better.
- Approach A is also about 4x faster to run than the baseline, and roughly 500x faster than reconstructing the image first (Approach B), since it skips the costly OMP optimization step entirely.
- Reconstructing the image before classifying (Approach B) is the slowest and least accurate option here — for this task, going straight from compressed measurements to a prediction is the better strategy.

## Tech Stack

- **Python 3.8+**
- **NumPy / SciPy** – compressed sensing math, random projections, OMP
- **scikit-learn** – LDA classifier
- **Matplotlib** – visualizations
- **Jupyter Notebook**

## Project Structure

```
compressed-sensing-mnist-classification/
├── CS_Application.ipynb                                  # main notebook: all 3 strategies, side by side
├── Direct Classification in the Compressed Domain.pdf     # written report with theory and full results
├── requirements.txt
└── README.md
```

## Installation

```bash
# 1. Clone the repository
git clone https://github.com/Soukaina009/compressed-sensing-mnist-classification.git
cd compressed-sensing-mnist-classification

# 2. Install dependencies
pip install -r requirements.txt
```

## Usage

```bash
jupyter notebook CS_Application.ipynb
```

The notebook loads MNIST, runs all three strategies, and prints the accuracy and timing for each. It also plots example digits before and after compression, so you can see what the compressed measurements look like.

## Report

I wrote a full report for this project: `Direct Classification in the Compressed Domain.pdf`. It covers the theory behind compressed sensing, including the Restricted Isometry Property (RIP) and the sparsity assumptions, along with the complete experimental setup and analysis of the results above.

## Author

Built by **Soukaina**, Master's student in AI for the Digital Economy and Management.
[GitHub](https://github.com/Soukaina009) · [LinkedIn](https://www.linkedin.com/in/soukaina-zemzam-585b8a3aa/?lipi=urn%3Ali%3Apage%3Ad_flagship3_profile_view_base%3BEMOBq%2F32RqGeLJ3s2tgDYQ%3D%3D) · [Email](zemzamsoukaina@gmail.com)

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
2. **Compressed Sensing – Direct Classification**: a random Gaussian projection reduces each image from 784 to 250 measurements ($y = \Phi x$). LDA then classifies directly on these 250 compressed values, with no reconstruction step.
3. **Compressed Sensing – Reconstruction (OMP)**: the same 250 compressed measurements are used, but the image is first reconstructed with Orthogonal Matching Pursuit (OMP) before LDA classifies it.

The compression relies on the fact that MNIST images are sparse in the wavelet domain (Haar / Daubechies), which is what makes accurate compressed measurement possible.

## Results

| Strategy | Input Dimension | Compression | Test Accuracy | Speed |
|---|---|---|---|---|
| Baseline (raw pixels) | 784 | 0% | **83.80%** | Standard |
| CS – Direct Classification | 250 | 68.2% | 81.50% – 82.50% | Fast (no reconstruction step) |
| CS – Reconstruction (OMP + LDA) | 250 → 80 coefficients | 68.2% | ~80.00% | Slow (OMP reconstruction is costly) |

**Main takeaways:**

- The direct classification approach cuts data volume by close to 70% and loses only about 1.5 points of accuracy compared to the baseline.
- Skipping reconstruction and classifying the compressed data directly is both faster and more accurate than reconstructing the image first.

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

For the full theory behind this project, including the Restricted Isometry Property (RIP), the sparsity assumptions, and the complete experimental setup, see `Direct Classification in the Compressed Domain.pdf` in this repository.

## Author

Built by **Zemzam Soukaina**, Master's student in AI for the Digital Economy and Management.
[GitHub](https://github.com/Soukaina009) · [LinkedIn](https://www.linkedin.com/in/soukaina-zemzam-585b8a3aa/?lipi=urn%3Ali%3Apage%3Ad_flagship3_profile_view_base%3BEMOBq%2F32RqGeLJ3s2tgDYQ%3D%3D) · [Email](zemzamsoukaina@gmail.com)

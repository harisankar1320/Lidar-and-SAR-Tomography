# Tomographic Laser and Radar Sensing

Implementation of LiDAR waveform processing and Tomographic SAR (TomoSAR) reconstruction for tropical forest structure analysis using airborne datasets from the NASA AfriSAR campaign over Lopé National Park, Gabon.

The project investigates the capabilities of LiDAR and multi-baseline SAR tomography for reconstructing vertical vegetation structure and compares the performance of both sensing techniques along a common forest transect.

---

## Overview

Forest vertical structure plays an important role in biomass estimation, carbon monitoring, and ecosystem assessment. This project processes airborne **NASA LVIS LiDAR** and **UAVSAR L-band TomoSAR** datasets to reconstruct forest profiles and evaluate the strengths and limitations of radar tomography.

The workflow consists of:

- LiDAR waveform processing
- Near-nadir shot selection
- DEM referencing
- SAR coherence estimation
- Capon beamforming
- LiDAR–TomoSAR comparison

---

## Dataset

The datasets were acquired during the **NASA AfriSAR campaign** over **Lopé National Park, Gabon**.

Data used:

- NASA LVIS LiDAR waveforms
- UAVSAR L-band multi-baseline SAR stack
- Digital Elevation Model (DEM)

---

# Methodology

## LiDAR Processing

The LiDAR workflow consists of:

- Reading LVIS waveform data
- Selecting near-nadir observations
- Converting waveform samples into range
- Referencing measurements to the DEM
- Normalizing waveform amplitudes
- Generating a forest structure transect

### Range Conversion

The LiDAR samples represent two-way travel time.

\[
R=\frac{ct}{2}
\]

where

- \(c\) = speed of light
- \(t\) = measured travel time

---

## TomoSAR Processing

The TomoSAR workflow includes:

- Matching SAR pixels with the LiDAR transect
- Estimating coherence matrices
- Applying diagonal loading
- Computing Capon beamforming
- Referencing reconstructed profiles to the DEM

### Signal Model

The multi-baseline SAR observations are represented by

\[
\mathbf{y}=A(z)\gamma+\mathbf{w}
\]

where

- **y** – SAR observation vector
- **A(z)** – steering matrix
- **γ** – vertical reflectivity profile
- **w** – additive noise

---

### Coherence Estimation

The normalized covariance (coherence) is estimated as

\[
\hat{C}_{n,m}=
\frac{\sum_{l=1}^{L}y_l^n(y_l^m)^*}
{\sqrt{\sum_{l=1}^{L}|y_l^n|^2
\sum_{l=1}^{L}|y_l^m|^2}}
\]

where

- \(L\) is the coherence window size.

---

### Capon Beamforming

The vertical power spectrum is estimated using the Capon beamformer

\[
P(z)=
\frac{1}
{\mathbf{a}^{H}(z)\mathbf{R}^{-1}\mathbf{a}(z)}
\]

where

- **R** is the covariance matrix
- **a(z)** is the steering vector

This method provides improved vertical resolution compared to conventional beamforming by incorporating the covariance information into the filter design.

---

# Processing Workflow

```text
LiDAR
│
├── Read waveform
├── Near-nadir selection
├── Range conversion
├── DEM referencing
├── Waveform normalization
└── Forest transect

            ↓

      LiDAR Profile

            ↑

TomoSAR
│
├── Pixel matching
├── Coherence estimation
├── Diagonal loading
├── Capon beamforming
├── DEM referencing
└── Vertical reflectivity profile

            ↓

Comparison of LiDAR and TomoSAR
```

---

# Results

## LiDAR

- Successfully extracted and visualized waveform returns.
- Generated near-nadir forest transects.
- Clearly distinguished forest, meadow, and water regions.
- Produced reliable vertical vegetation profiles.

### Example

<p align="center">
<img src="figures/lidar_transect.png" width="800">
</p>

---

## TomoSAR

The Capon beamforming implementation produced vertical reflectivity profiles; however, reconstruction quality varied significantly across the scene.

Observations:

- Low-coherence regions produced noisy reconstructions.
- Beamforming failed to recover reliable vertical structure in several locations.
- Increasing the coherence window from **15×15** to **25×25** reduced noise but also smoothed important forest details.

### Example

<p align="center">
<img src="figures/capon_power.png" width="800">
</p>

---

## Coherence Analysis

Coherence matrices were analyzed at multiple locations to investigate reconstruction quality.

Higher coherence generally resulted in better TomoSAR profiles, while decorrelated regions exhibited poor beamforming performance.

<p align="center">
<img src="figures/coherence_matrix.png" width="650">
</p>

---

## LiDAR vs TomoSAR

The comparison highlighted the strengths and limitations of each sensing technique.

### LiDAR

✔ High vertical accuracy

✔ Clear canopy representation

✔ Reliable forest profile

### TomoSAR

✔ Capable of estimating general forest structure

✖ Sensitive to coherence

✖ Noisy in decorrelated regions

✖ Lower reconstruction quality than LiDAR for this dataset

---

# Repository Structure

```text
.
├── Project-TLRS.ipynb
├── README.md
├── figures/
│   ├── lidar_transect.png
│   ├── coherence_matrix.png
│   ├── capon_power.png
│   └── comparison.png
└── data/
```

---

# Technologies

- Python
- NumPy
- SciPy
- h5py
- Matplotlib
- Jupyter Notebook

---

# Future Improvements

- Adaptive coherence window selection
- Regularized inversion methods
- MUSIC and compressive sensing tomography
- Improved covariance estimation
- Quantitative validation against LiDAR-derived canopy height

---

# References

- Aghababaei, H. et al. (2020). *Forest SAR Tomography: Principles and Applications*. IEEE Geoscience and Remote Sensing Magazine.
- NASA AfriSAR Campaign.
- Karlsruhe Institute of Technology – Tomographic Laser and Radar Sensing Course.

---

## Authors

**Harisankar Harikumar**

**Dorsa Amini**

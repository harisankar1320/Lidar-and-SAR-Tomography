# Tomographic Laser and Radar Sensing

Implementation of **LiDAR waveform processing** and **Tomographic Synthetic Aperture Radar (TomoSAR)** reconstruction for tropical forest structure analysis using airborne datasets from the **NASA AfriSAR campaign** over Lopé National Park, Gabon.

The project investigates the ability of LiDAR and multi-baseline SAR tomography to reconstruct vertical forest structure and compares the performance of both sensing techniques along a common transect.

---

## Project Overview

Forest vertical structure is an important parameter for biomass estimation, carbon monitoring, and ecosystem analysis. While LiDAR directly measures canopy height with high accuracy, Tomographic SAR estimates the vertical reflectivity profile using multiple SAR acquisitions.

This project implements processing workflows for both datasets and evaluates their performance by comparing reconstructed forest profiles.

---

## Dataset

The project uses airborne datasets acquired during the **NASA AfriSAR Campaign**.

**Study Area**
- Lopé National Park, Gabon

**Datasets**
- NASA LVIS LiDAR Waveforms
- UAVSAR L-band Multi-baseline SAR Stack
- Digital Elevation Model (DEM)

---

# Methodology

## LiDAR Processing

The LiDAR workflow consists of:

- Reading LVIS waveform data
- Selecting near-nadir measurements
- Converting waveform samples to range
- Referencing measurements to the DEM
- Normalizing waveform amplitudes
- Generating forest structure transects

### Range Conversion

The recorded waveform represents the two-way travel time of the laser pulse.

```math
R=\frac{ct}{2}
```

where

- **R** = Range
- **c** = Speed of light
- **t** = Two-way travel time

---

## TomoSAR Processing

The TomoSAR workflow includes:

- Matching SAR pixels to the LiDAR transect
- Estimating coherence matrices
- Applying diagonal loading
- Implementing Capon Beamforming
- Referencing reconstructed profiles to the DEM
- Comparing TomoSAR and LiDAR transects

---

### Multi-baseline Signal Model

The SAR observations are modeled as

```math
\mathbf{y}=A(z)\gamma+\mathbf{w}
```

where

- **y** – Observation vector
- **A(z)** – Steering matrix
- **γ** – Vertical reflectivity profile
- **w** – Additive noise

Recovering the reflectivity profile is an inverse problem solved using Capon Beamforming.

---

### Coherence Estimation

The normalized coherence between two SAR acquisitions is estimated as

```math
\hat{C}_{n,m}=
\frac{\sum_{l=1}^{L} y_l^{n}(y_l^{m})^{*}}
{\sqrt{\sum_{l=1}^{L}|y_l^{n}|^{2}
\sum_{l=1}^{L}|y_l^{m}|^{2}}}
```

where

- **L** = Window size
- **y** = Complex SAR observations

A diagonal loading factor was added to stabilize covariance estimation before beamforming.

---

### Capon Beamforming

The Capon estimator reconstructs the vertical reflectivity spectrum using

```math
P(z)=
\frac{1}
{\mathbf{a}^{H}(z)\mathbf{R}^{-1}\mathbf{a}(z)}
```

where

- **R** = Covariance matrix
- **a(z)** = Steering vector
- **H** = Hermitian transpose

Compared to conventional beamforming, the Capon estimator provides improved vertical resolution by incorporating the covariance information into the filter design.

---

# Processing Workflow

```text
LiDAR Processing
│
├── Read waveform
├── Near-nadir selection
├── Range conversion
├── DEM referencing
├── Waveform normalization
└── Forest transect
        │
        ▼
 LiDAR Vertical Profile


TomoSAR Processing
│
├── Pixel matching
├── Coherence estimation
├── Diagonal loading
├── Capon beamforming
├── DEM referencing
└── Vertical reflectivity profile
        │
        ▼
 TomoSAR Vertical Profile
        │
        ▼
 LiDAR vs TomoSAR Comparison
```

---

# Results

## LiDAR Results

The LiDAR processing successfully reconstructed the vertical forest structure.

The generated transect clearly distinguished

- Forest
- Meadows
- Water bodies

using their characteristic waveform responses. The resulting vertical profiles provided reliable canopy structure information.

---

## TomoSAR Results

The implemented Capon beamforming algorithm reconstructed the vertical reflectivity profile from the multi-baseline SAR stack.

However, the reconstruction quality varied considerably across the scene.

Key observations include:

- Low-coherence regions produced noisy vertical profiles.
- Several areas showed weak reflectivity due to insufficient coherence.
- Increasing the coherence window from **15 × 15** to **25 × 25** reduced noise but also smoothed fine forest structures.
- Beamforming performance depended strongly on the quality of the estimated covariance matrix.

---

## LiDAR vs TomoSAR Comparison

The comparison highlighted the complementary characteristics of both sensing techniques.

### LiDAR

✔ High vertical accuracy

✔ Reliable canopy representation

✔ Clear distinction between forest, meadow and water

### TomoSAR

✔ Able to estimate the general vertical forest structure

✔ Useful over coherent regions

✖ Reconstruction degraded in decorrelated areas

✖ Sensitive to coherence estimation and window size

---

# Repository Structure

```text
.
├── Project-TLRS.ipynb
├── README.md
├── figures/
│   ├── lidar_waveform.png
│   ├── lidar_transect.png
│   ├── coherence_matrix.png
│   ├── capon_power.png
│   └── comparison.png
└── data/
```

---

# Technologies Used

- Python
- NumPy
- SciPy
- Matplotlib
- h5py
- Jupyter Notebook

---

# Future Improvements

- MUSIC-based tomography
- Sparse reconstruction methods
- Adaptive coherence window selection
- Improved covariance estimation
- Quantitative comparison with LiDAR-derived canopy heights

---

# References

1. Aghababaei, H., et al. (2020). *Forest SAR Tomography: Principles and Applications*. IEEE Geoscience and Remote Sensing Magazine.

2. NASA AfriSAR Campaign.

3. Karlsruhe Institute of Technology – Tomographic Laser and Radar Sensing.

---

## Authors

**Harisankar Harikumar**

**Dorsa Amini**

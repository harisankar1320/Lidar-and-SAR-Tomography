# Tomographic Laser and Radar Sensing

This project investigates the reconstruction of tropical forest structure using airborne LiDAR and Tomographic SAR (TomoSAR) data acquired during the NASA AfriSAR campaign over Lopé National Park, Gabon.

The objective was to process both datasets independently, generate vertical forest profiles, and compare their capabilities for representing forest structure.

---

## Project Objectives

- Process NASA LVIS LiDAR waveforms
- Generate forest transects from LiDAR data
- Compute coherence matrices from SAR image stacks
- Implement Capon beamforming for TomoSAR reconstruction
- Compare LiDAR and TomoSAR vertical profiles
- Evaluate the limitations of TomoSAR using coherence analysis

---

## Methods

### LiDAR Processing

- Read LVIS waveform data
- Select near-nadir measurements
- Convert waveform samples to range
- Reference waveforms using DEM elevation
- Normalize waveform amplitudes
- Generate forest transects

### TomoSAR Processing

- Match SAR pixels to the LiDAR transect
- Estimate coherence matrices using local windows
- Apply Capon beamforming
- Reference reconstructed profiles to the DEM
- Investigate the influence of different coherence window sizes

---

## Results

### LiDAR

The LiDAR processing successfully reconstructed the vertical forest structure. The generated transect clearly distinguished forests, meadows, and water bodies through their characteristic waveform responses.

### TomoSAR

The Capon beamforming implementation produced vertical reflectivity profiles; however, the reconstruction quality was limited. Several regions showed weak or noisy responses due to insufficient coherence, making interpretation difficult.

Using a larger coherence window (25 × 25) reduced noise but also removed fine structural details, demonstrating the trade-off between stability and spatial resolution.

### Comparison

The comparison highlighted the strengths and weaknesses of both sensing techniques:

- LiDAR produced clearer and more reliable vertical vegetation profiles.
- TomoSAR captured general structural information but struggled in decorrelated regions.
- Low coherence significantly degraded the beamforming results.
- Coherence analysis helped explain locations where the TomoSAR reconstruction failed.

---

## Technologies

- Python
- NumPy
- SciPy
- Matplotlib
- h5py
- Jupyter Notebook

---

## Repository Structure

```
.
├── Project-TLRS.ipynb
├── README.md
└── figures/
```

---

## Example Outputs

- LiDAR waveform visualization
- Forest transect
- Coherence matrices
- Capon power spectrum
- LiDAR vs TomoSAR comparison

---

## Dataset

The project uses airborne LiDAR (NASA LVIS) and L-band UAVSAR datasets collected during the NASA AfriSAR campaign over Lopé National Park, Gabon.

---

- Harisankar Harikumar
- Dorsa Amini

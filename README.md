# Data Acquisition and Analysis of the Cataclysmic Variable Binary — 1RXS J064434.5+334451

**Master's Dissertation Work | Aritra Roy | M.Sc. Physics (Spectroscopy)**
**Maharaja Sayajirao University of Baroda | 2023–25**

> This dissertation work presents a photometric and orbital analysis of a cataclysmic variable (CV) star system, Utilizing publicly available data from the Transiting Exoplanet Survey Satellite (TESS) and the Wide-field Infrared Survey Explorer (WISE). The study investigates brightness variability and periodic behaviour.

---

## Overview

Cataclysmic Variable (CV) stars are close, interacting binary systems in which a white dwarf accretes matter from a Roche-lobe-overflowing low-mass companion (typically a red dwarf). This project investigates the photometric variability of **1RXS J064434.5+334451**, a known high-inclination eclipsing CV, composed of a white dwarf accreting material from a low-mass main-sequence companion, using multi-wavelength space-based survey data.

The analysis pipeline covers:
- Light curve extraction from **TESS** (optical, 2-min cadence) and **WISE/NEOWISE** (infrared W1 & W2 bands)
- Data cleaning: normalisation, NaN removal, outlier rejection, and polynomial detrending
- Period detection via the **Lomb-Scargle Periodogram**
- Visual confirmation through **phase-folded light curves**

**Key result:** Orbital period of **P_orb = 0.26905 days (~6.46 hours)**, consistent with literature values of 0.26937446 d (Hernández Santisteban et al. 2017) and 0.26937447(4) d (Sing et al. 2007).

---

## Repository Structure

```
CV-star-light-curve-analysis/
│
├── notebooks/
│   ├── 00_LOMS.ipynb           # 
│   └── 01_LOMS_Addendum.ipynb  # 
│
├── references/
│   ├── 01
│   ├── 02
│   ├── 03
│   ├── 04
│   ├── 05
│   └── 06
│
├── report/
│   ├── presentation.pdf        # presentation as part of dissertation defence
│   ├── thesis.pdf              # Dissertation thesis
│   └── summary.pdf             #
│
├── results/
│   ├── figures/              # Output plots (light curves, periodograms, phase curves)
│   └── tables/               # Period tables and numerical results
│
├── LICENSE
└── README.md
```

---

## Target Object

| Parameter | Value |
|---|---|
| Object ID | 1RXS J064434.5+334451 |
| Object Type | Cataclysmic Variable (CV) |
| RA | 06h 44m 34.637s |
| Dec | +33° 44′ 56.615″ |
| TESS Input Catalog ID | TIC 240122720 |
| ROSAT All-Sky Survey ID | 1RXS J064434.5+334451 |
| Derived P_orb | 0.26905 days (~6.46 hr) |
| Literature P_orb 1 | 0.26937446 days (Hernández Santisteban et al. 2017) |
| Literature P_orb 2 | 0.26937447(4) days (Sing et al. 2007) |

---

## Notebooks Guide

| Notebook | Description |
|---|---|
| `00_LOMS.ipynb` |  |
| `01_LOMS_Addendum.ipynb` |  |

---

## Methodology Summary

### 1. Data Sources
- **TESS** (Transiting Exoplanet Survey Satellite): broadband optical (0.6–1.0 µm), 2-minute cadence, accessed via MAST archive using `lightkurve`
- **WISE/NEOWISE**: (Wide-field Infrared Survey Explorer)/ (Near-Earth Object WISE): mid-infrared W1 (3.4 µm) and W2 (4.6 µm) bands, ~185-minute cadence, accessed via IRSA using `astroquery.ipac.irsa`

### 2. Data Cleaning
- TESS: 2-minute cadence (high) → finely sampled data → mean-normalisation (sensitive to outliers) → NaN removal → 3σ outlier clipping
- WISE: 185-minute cadence (low) → sparsely sampled data → median-normalisation (less sensitive to outliers and irregular sampling) → cubic polynomial detrending to remove long-term secular trends

### 3. Period Analysis (Lomb-Scargle)
The Lomb-Scargle Periodogram is used instead of the classical FFT-based approach because WISE data is irregularly sampled. Implemented via `astropy.timeseries.LombScargle`. Frequencies from P ~ 0.1 d to P ~ 30 d were searched.

**Significant peaks detected:**

| Dataset | Peak 1 (days) | Peak 2 (days) |
|---|---|---|
| TESS | **0.13469** | 0.26888 |
| WISE W1 | **0.13444** | 0.26888 |
| WISE W2 | 6.049 | 0.26888 |

### 4. Period Identification
Phase-folding at P = 0.1345 d produced a **split eclipse dip** at phases 0 and 1, indicating this is a harmonic (half the true period). Following Osaki (1974), the presence of both P and 2P in the periodogram confirms the true orbital period is **2P = 0.26905 days**. This is reinforced by the two clear eclipse dips in the 2P-folded TESS light curve.

---

## Results

- **Derived orbital period:** 0.26905 days (6.4572 hours)
- **Agreement with literature:** within 0.001 d of both Hernández Santisteban et al. (2017) and Sing et al. (2007)
- **System classification confirmed:** high-inclination eclipsing CV binary (white dwarf + low-mass companion)
- **Eclipse morphology:** V-shaped primary eclipse clearly resolved; secondary eclipse present at phase ≈ 1.5 in 2P-folded curve
- **Multi-wavelength consistency:** 0.1345 d signal reproduced in WISE W1, confirming intrinsic astrophysical origin

---

## Installation

```bash
git clone https://github.com/Aritra-Roy-1/cv-binary-analysis.git
cd cv-binary-analysis
pip install -r requirements.txt
```

Or with conda:
```bash
conda env create -f environment.yml
conda activate cv-analysis
```

Then launch Jupyter:
```bash
jupyter notebook notebooks/
```

---

## Dependencies

See `requirements.txt`. Key packages:

| Package | Purpose |
|---|---|
| `lightkurve` | TESS light curve download and preprocessing |
| `astroquery` | WISE/NEOWISE data retrieval via IRSA |
| `astropy` | Lomb-Scargle periodogram, coordinate handling, time conversion |
| `numpy` | Numerical operations |
| `matplotlib` | All visualisations |
| `pandas` | Tabular data handling |
| `scipy` | Polynomial fitting, spline interpolation |
| `tglc` | Alternative TESS FFI light curve access |

---

## References

1. Hellier, C. (2001). *Cataclysmic Variable Stars: How and Why They Vary*. Springer.
2. Wenger, M. et al. (2000). The SIMBAD astronomical database. *A&AS*, 143, 9–22.
3. Wright, E. L. et al. (2010). The WISE mission description. *AJ*, 140, 1868.
4. Ricker, G. R. et al. (2015). Transiting Exoplanet Survey Satellite (TESS). *JATIS*, 1, 014003.
5. VanderPlas, J. T. (2018). Understanding the Lomb–Scargle Periodogram. *ApJS*, 236, 16.
6. Osaki, Y. (1974). The disk instability model of dwarf novae. *PASJ*, 26, 420–438.
7. Hernández Santisteban, J. V. et al. (2017). Doppler tomography and photometry of 1RXS J064434.5+334451. *MNRAS*, 464, 104–114.
8. Sing, D. K. et al. (2007). Discovery of a bright eclipsing cataclysmic variable. *A&A*, 474, 951–960.

---

## Supervisors

- **Dr. Vishal Joshi** — Scientist, Astronomy and Astrophysics Division, Physical Research Laboratory, Ahmedabad
- **Dr. Kanti Jotania** — Associate Professor, Department of Physics, M.S. University of Baroda

---

## License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.

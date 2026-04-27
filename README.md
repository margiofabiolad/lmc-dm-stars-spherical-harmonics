# lmc-dm-stars-spherical-harmonics
Spherical harmonic analysis of the MW stellar halo response to the LMC's infall, using N-Body simulations

# Kinematic Analysis of the Milky Way Stellar Halo: LMC-Induced Dark Matter Wake

## Overview

This project implements and extends the spherical harmonic expansion methodology from
[Cunningham et al. (2020)](https://arxiv.org/abs/2006.08621) to characterize the kinematic
response of the Milky Way (MW) stellar halo to the infall of the Large Magellanic Cloud (LMC).

The analysis is applied to N-body simulations from Garavito-Camargo et al. (2019), which model
the LMC's first infall into the MW and the resulting dark matter (DM) density wake. Two
simulations are compared: one with and one without the LMC's gravitational influence.

---

## Scientific Context

The LMC is the most massive satellite of the Milky Way (~10¹¹ M☉) and is likely on its first
infall. Its gravitational influence induces a **dark matter density wake** in the MW halo,
producing two distinct kinematic components:

- **Transient Response**: a local overdensity trailing the LMC's orbit (classical dynamical friction wake)
- **Collective Response**: a global halo response creating a large-scale overdensity in the north and net motion of the MW disk toward the LMC-MW barycenter

These signatures can be quantified using **spherical harmonic expansion (SHE)** of the velocity
fields, which separates kinematic perturbations by angular scale.

---

## What I Did

### 1. Independent Implementation of the SHE Pipeline

Starting from raw N-body simulation data (1 million particles, positions in kpc and velocities in km/s):

- Converted Cartesian coordinates (x, y, z) → spherical coordinates (r, θ, φ) using `astropy`
- Converted Cartesian velocities (vx, vy, vz) → spherical velocity components (vR, vθ, vφ)
- Applied radial shells at **50, 60, 70, 75, 80, and 100 kpc** from the Galactic Center
- Computed HEALPix maps of mean velocities in each shell using `healpy`
- Calculated angular power spectra C_ℓ via `hp.anafast`

### 2. Comparison of Two Simulations

| Simulation | Description |
|---|---|
| `rand_mwb1_110` | MW-only halo (no LMC) |
| `rand_mwlmc5b0_110` | MW halo + LMC infall (M_LMC = 1.1 × 10¹¹ M☉) |

The comparison confirms that the LMC is responsible for the observed kinematic signatures.

### 3. Extension: Overdensity Power Spectrum

Beyond Cunningham et al. (2020), I also computed the **angular power spectrum of the stellar
overdensity field** — not only the velocity fields. This adds a spatial dimension to the
kinematic analysis.

### 4. Recovery of Published Results

My results reproduce the key signatures reported in Cunningham et al. (2020):

| Signature | ℓ mode | Velocity Component | Physical Origin |
|---|---|---|---|
| Collective Response | ℓ = 0 | ⟨vθ⟩ | Net upward motion of halo w.r.t. disk |
| Collective Response | ℓ = 1 | ⟨vR⟩ | Radial velocity dipole, increases with distance |
| Transient Response | ℓ = 2 | ⟨vφ⟩ | Stars trapped near LMC's orbit |

All signatures increase in amplitude with Galactocentric distance, consistent with the paper.

---

## Results

### Angular Power Spectra at 50–100 kpc

![Angular Power Spectra](cls_5075100-comparacion)

*Top left: overdensity power spectrum (original extension). Top right: ⟨vR⟩. Bottom left: ⟨vθ⟩. Bottom right: ⟨vφ⟩. All computed for the MW+LMC simulation at multiple Galactocentric distances.*

The dominant modes match theoretical predictions:
- **ℓ = 1 in ⟨vR⟩**: radial dipole from the Collective Response, growing with distance
- **ℓ = 0 in ⟨vθ⟩**: monopole from net halo motion, strongest at 100 kpc
- **ℓ = 2 in ⟨vφ⟩**: Transient Response signature near the LMC's position

---

## Technical Stack

| Tool | Purpose |
|---|---|
| `Python 3.10` | Core language |
| `Pandas` | Data loading and manipulation (1M+ rows) |
| `NumPy` | Vectorized computation |
| `Astropy` | Coordinate transformations, unit handling |
| `Healpy` | HEALPix pixelization, Mollweide projections, power spectra |
| `Matplotlib` | All visualizations |
| `Google Colab` | Cloud compute environment |

---

## Data

The simulations used are from [Garavito-Camargo et al. (2019)](https://doi.org/10.3847/1538-4357/ab32eb).
Raw data files are not included in this repository due to size, but can be requested from the
original authors or accessed via their public repository.

---

## References

- Cunningham, E. C. et al. (2020). *Quantifying the Stellar Halo's Response to the LMC's Infall with Spherical Harmonics*. [arXiv:2006.08621](https://arxiv.org/abs/2006.08621)
- Garavito-Camargo, N. et al. (2019). *Hunting for the Dark Matter Wake Induced by the LMC*. ApJ, 884, 51. [doi:10.3847/1538-4357/ab32eb](https://doi.org/10.3847/1538-4357/ab32eb)

---


## Author
**Margionet Fabiola Díaz** — Licenciada en Física, Universidad de Los Andes, Mérida, Venezuela  
[GitHub](https://github.com/margiofabiolad)

*This project was developed during my doctoral research in Astronomy at the 
Instituto de Astronomía Teórica y Experimental (IATE), UNC, under the supervision 
of Dr. Mariano Domínguez.*

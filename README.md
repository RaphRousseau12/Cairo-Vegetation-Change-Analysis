# Cairo Vegetation Change Analysis (NDVI)

> **Project status: Work in progress.** This repository is still under development. The analysis, code, visualizations, and documentation are not final and may change as the project progresses.

## Overview

This project explores vegetation patterns and their changes in **Cairo, Egypt**, using multispectral satellite imagery from **Copernicus Sentinel-2 Level-2A (L2A)**.

The current workflow compares two August scenes:
- **11 August 2021**
- **5 August 2026**

The main objective is to calculate and compare the **Normalized Difference Vegetation Index (NDVI)** in order to examine the spatial distribution of vegetation and its evolution between the two dates.

The notebook also explores complementary spectral indices and image visualizations, including the Normalized Difference Built-up Index (NDBI) and the Normalized Difference Water Index (NDWI).

## Data

The imagery was obtained from the **Copernicus Browser** and consists of Sentinel-2 L2A spectral-band GeoTIFF files.

The notebook currently loads the following bands for both dates:

| Band | Sentinel-2 band name | Main use in this project |
|---|---|---|
| B2 | Blue | Natural-colour composite |
| B3 | Green | Natural-colour composite and NDWI |
| B4 | Red | Natural-colour composite and NDVI |
| B8 | Near Infrared (NIR) | False-colour composite and NDVI |
| B11 | Short-Wave Infrared (SWIR) | NDBI |
| B12 | Short-Wave Infrared (SWIR) | Loaded for spectral exploration |

Acquisition dates and filenames are currently defined directly in the notebook. See [DATA_SOURCES.md](DATA_SOURCES.md) for more information about the data and the expected local folder structure.

**The satellite rasters are not included in this repository.** They can be large files and should be downloaded separately from the Copernicus Browser. Do not commit raw imagery unless you have checked the applicable data terms and repository size constraints.

## Current workflow

The Jupyter Notebook (`notebooks/cairo.ipynb`) currently contains the following steps:

1. **Load spectral bands** for 2021 and 2026 using Rasterio.
2. **Inspect raster values** and create pixel-value distributions.
3. **Visualize individual bands** in grayscale.
4. **Create RGB composites**:
   - Natural colours: B4–B3–B2.
   - False colours: B8–B4–B3.
5. **Calculate NDVI** using the red and near-infrared bands.
6. **Compare NDVI** between 2021 and 2026 and map the difference.
7. **Explore vegetation areas** using an NDVI threshold.
8. **Calculate NDBI** to explore built-up areas.
9. **Calculate NDWI** as part of the water masking used in the change-typology workflow.
10. **Classify vegetation change** into five categories: strong loss, slight loss, stable, slight gain, and strong gain. The current classification uses the median and a robust dispersion estimate based on the median absolute deviation (MAD).

The workflow and interpretation are still being developed. The outputs should therefore be considered preliminary.

## NDVI

The NDVI is calculated from the near-infrared (NIR) and red bands:

\[
NDVI = \frac{NIR - Red}{NIR + Red}
\]

For Sentinel-2, this project uses:
- **NIR:** Band 8 (B8)
- **Red:** Band 4 (B4)

NDVI values theoretically range from -1 to +1. Higher positive values are generally associated with greener vegetation, while low or negative values can correspond to bare surfaces, built-up areas, water, or other non-vegetated surfaces. Interpretation depends on local conditions and image quality.

## Repository structure

The recommended repository layout is:

```text
cairo-ndvi/
├── README.md
├── DATA_SOURCES.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   └── cairo.ipynb
└── data/
    └── Browser_images/
        ├── 2021-08-11/
        │   ├── ... Sentinel-2 L2A band GeoTIFF files ...
        └── 2026-08-05/
            ├── ... Sentinel-2 L2A band GeoTIFF files ...
```

The `data/` directory is expected locally but is excluded from Git by the recommended `.gitignore` file. The notebook defines `PROJECT_DIR = Path.cwd().parent`, so open and run it from the `notebooks/` directory (or update the path logic if you use a different layout).

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/<your-repository>.git
cd <your-repository>
```

### 2. Create and activate a virtual environment

```bash
python -m venv .venv
```

On Windows:

```bash
.venv\Scripts\activate
```

On macOS or Linux:

```bash
source .venv/bin/activate
```

### 3. Install dependencies

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 4. Download and arrange the satellite data

Download the required Sentinel-2 L2A bands from the Copernicus Browser for the two acquisition dates listed in [DATA_SOURCES.md](DATA_SOURCES.md).

Place the GeoTIFF files in the matching `data/Browser_images/<date>/` directories. The filenames must match the paths referenced in the notebook, or the notebook paths must be updated.

### 5. Run the notebook

From the repository root:

```bash
jupyter notebook
```

Open `notebooks/cairo.ipynb`. Because the notebook currently sets its project directory to the parent of the working directory, run the notebook with `notebooks/` as the working directory. If your Jupyter environment starts in the repository root, adjust the `PROJECT_DIR` definition accordingly.

## Main technologies

- Python
- Jupyter Notebook
- Rasterio
- NumPy
- Pandas
- Matplotlib
- Copernicus Sentinel-2 L2A imagery

The full pinned Python environment is listed in `requirements.txt`.

## Current limitations and next steps

This is an unfinished project. Possible next steps include:

- Finalize and validate the preprocessing of the satellite rasters, including consistent spatial resolution, grid alignment, and NoData handling.
- Check that the two dates are comparable in terms of spatial coverage, atmospheric conditions, and processing.
- Review the water-masking and vegetation thresholds.
- Validate the NDVI change classification and its area calculations.
- Improve map layouts, legends, scale bars, and geographic context.
- Add a clear interpretation of the observed changes and their spatial distribution.
- Refactor repeated notebook code into reusable functions and make file paths configurable.

## Data source and attribution

Satellite imagery: **Copernicus Sentinel-2 L2A**, accessed through the **Copernicus Browser**.

Please consult [DATA_SOURCES.md](DATA_SOURCES.md) for data details and attribution guidance.

---

*This repository is a work in progress. Results and interpretations are preliminary.*

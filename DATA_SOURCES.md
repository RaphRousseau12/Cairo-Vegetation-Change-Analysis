# Data sources and documentation

## Satellite platform and product

The project notebook identifies the imagery as **Sentinel-2 Level-2A (L2A)**. The filenames explicitly contain `Sentinel-2_L2A` and the notebook loads the following Sentinel-2 bands:

- B2 — Blue
- B3 — Green
- B4 — Red
- B8 — Near Infrared (NIR)
- B11 — Short-Wave Infrared (SWIR)
- B12 — Short-Wave Infrared (SWIR)

The data were accessed through the **Copernicus Browser**. The notebook itself does not include the original Copernicus product identifiers or complete product metadata, so these should be recorded here when available (for example, product ID, tile, sensing time, and processing baseline).

## Acquisition dates used in the notebook

| Comparison year | Acquisition date used in the notebook |
|---|---|
| 2021 | 11 August 2021 |
| 2026 | 5 August 2026 |

These are the dates encoded in the notebook's local file paths. Confirm the dates and product metadata against the downloaded source files before publishing final results.

## Bands and indices

| Index / composite | Formula or band combination | Bands |
|---|---|---|
| NDVI | (B8 - B4) / (B8 + B4) | NIR, Red |
| NDBI | (B11 - B8) / (B11 + B8) | SWIR, NIR |
| NDWI | (B3 - B8) / (B3 + B8) | Green, NIR |
| Natural-colour composite | RGB = B4, B3, B2 | Red, Green, Blue |
| False-colour composite | RGB = B8, B4, B3 | NIR, Red, Green |

The notebook applies a near-infrared threshold to mask water in its NDVI and NDBI calculations, and later uses an NDWI-based mask in the change-typology workflow. These are current implementation choices, not a validated final classification method.

## Local data directory expected by the notebook

Place the downloaded GeoTIFF files in this structure:

```text
data/
└── Browser_images/
    ├── 2021-08-11/
    │   ├── 2021-08-11-00_00_2021-08-11-23_59_Sentinel-2_L2A_B02_(Raw).tiff
    │   ├── 2021-08-11-00_00_2021-08-11-23_59_Sentinel-2_L2A_B03_(Raw).tiff
    │   ├── 2021-08-11-00_00_2021-08-11-23_59_Sentinel-2_L2A_B04_(Raw).tiff
    │   ├── 2021-08-11-00_00_2021-08-11-23_59_Sentinel-2_L2A_B08_(Raw).tiff
    │   ├── 2021-08-11-00_00_2021-08-11-23_59_Sentinel-2_L2A_B11_(Raw).tiff
    │   └── 2021-08-11-00_00_2021-08-11-23_59_Sentinel-2_L2A_B12_(Raw).tiff
    └── 2026-08-05/
        ├── 2026-08-05-00_00_2026-08-05-23_59_Sentinel-2_L2A_B02_(Raw).tiff
        ├── 2026-08-05-00_00_2026-08-05-23_59_Sentinel-2_L2A_B03_(Raw).tiff
        ├── 2026-08-05-00_00_2026-08-05-23_59_Sentinel-2_L2A_B04_(Raw).tiff
        ├── 2026-08-05-00_00_2026-08-05-23_59_Sentinel-2_L2A_B08_(Raw).tiff
        ├── 2026-08-05-00_00_2026-08-05-23_59_Sentinel-2_L2A_B11_(Raw).tiff
        └── 2026-08-05-00_00_2026-08-05-23_59_Sentinel-2_L2A_B12_(Raw).tiff
```

The names above are taken from the paths currently hard-coded in the notebook. If your downloaded files have different names, either rename them or update the notebook.

## Attribution

Data source: **Copernicus Sentinel-2 L2A**, accessed via the **Copernicus Browser**.

When publishing, retain the relevant Copernicus attribution and follow the applicable Copernicus data and product terms. Add the exact product IDs and access date(s) once they have been checked against the original downloads.

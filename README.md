# Flood-detection-of-Gurdaspur-Punjab
Flood/water-change detection for Gurdaspur (Punjab) using Sentinel-2 NDWI (cloud-masked) and Sentinel-1 VV/VH change, comparing dry vs monsoon. Data downloaded via Copernicus/Sentinel Hub, processed in Python, mapped in ArcMap.

---

##  README.md (GURDASPUR) — 

```md
# Flood / Water Change Detection — Gurdaspur (Punjab, India)

## 1) Project Summary
This project detects **water / flood-like change** inside the **Gurdaspur** ROI by comparing:
- **T1 (Dry baseline):** 2025-04-01 to 2025-04-30
- **T2 (Monsoon / flood-like):** 2025-08-15 to 2025-09-15

Final outputs are GIS-ready and were checked in **ArcMap**.

## 2) Platforms Used (by workflow step)
- **Visual Studio Code:** Automated data download + tiling + mosaicking using **Copernicus Data Space / Sentinel Hub API**
- **JupyterLab:** Coregistration + change detection processing
- **ArcGIS Desktop (ArcMap):** Final visualization, raster value checking, and final mask cleaning (NoData → 0)

## 3) ROI (Area of Interest)
- ROI file: `data/roi_1.geojson`
- CRS: **EPSG:4326 (WGS84 lat/long)**
- ROI name: **Gurdaspur, Punjab, India**

## 4) Datasets Used (Satellite + Bands)
### Optical
- **Satellite:** Sentinel-2
- **Product:** Sentinel-2 L2A (surface reflectance)
- **Bands downloaded:**
  - **B03 (Green)**
  - **B08 (NIR)**
  - **SCL (Scene Classification Layer)**
  - **cloudMask (created band)**: 1 = clear, 0 = masked (cloud/shadow/snow/no-data)

### SAR
- **Satellite:** Sentinel-1
- **Product:** Sentinel-1 GRD
- **Channels downloaded:**
  - **VV**
  - **VH**

## 5) Resolution Used
- Target resolution used in code: **DESIRED_RES_M = 10.0 m/pixel**
- Output resolution:
  - Sentinel-2 output: ~10 m
  - Sentinel-1 output: ~10 m

## 6) Cloud Handling (Optical Only)
Two layers were used:
1) **Scene filtering + mosaicking choice**
   - `mosaicking_order = LEAST_CC`
   - `maxcc = 0.2` (≤ 20% scene cloud cover when available)
2) **Pixel-level masking using SCL**
   - Cloud/invalid SCL classes were masked and written as nodata (-9999) for B03/B08
   - cloudMask band marks clear vs masked pixels

## 7) Reproducible Run Instructions (Start → End)

### A) Setup Environment
1) Create & activate Python environment (any standard method).
2) Install dependencies:
   ```bash
   pip install -r requirements.txt

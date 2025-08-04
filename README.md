# 🛰️ Drone Image Plot Labeling using EXIF GPS and Plot GeoJSON

This repository provides a Python tool for **automatically labeling drone-captured images** based on their **GPS coordinates** and **field plot geometries**.

Images are renamed using the **nearest plot number** derived from a `.geojson` file, enabling downstream analysis of spatially-tagged imagery and plot-level data alignment.

---

## 📌 What It Does

- Reads **GPS coordinates** from drone image EXIF metadata.
- Loads **plot geometry boundaries** from a `.geojson` file.
- Determines the closest plot to each image location using spatial distance.
- Renames each image by **prefixing it with the correct plot number** (e.g., `3_IMG_0045.JPG`).
- Optionally generates a **visual interactive map (HTML)** showing:
  - Plot boundaries
  - Image positions
  - Image-to-plot associations

---

## 📂 Example Folder Structure

```
/Plot_Labeling/
├── 2025-05-29_Transect/
│   └── Transect/
│       ├── IMG_0001.JPG
│       ├── IMG_0002.JPG
├── RRR2_ALLCOMPONENTS_ANDRADE_plots_20240809.geojson
├── label_by_gps.py
└── Plot_labeling_NEW/
    ├── 2025-05-29_Renamed/
    │   ├── 3_IMG_0001.JPG
    │   ├── 5_IMG_0002.JPG
    │   └── ...
    └── 2025-05-29_interactive_map.html
```

---

## 🧭 How It Works

1. For each `.JPG` in the input folder:
   - Extracts `GPSLatitude`, `GPSLongitude`, and direction info from EXIF metadata.
2. Loads `.geojson` file containing plot polygons and `Plot_No` field.
3. Computes the **closest plot polygon** to each image point using `shapely`.
4. Renames image with the format:  
   ```
   {Plot_No}_{OriginalFileName}.JPG
   ```
5. Copies the renamed image to the output folder.
6. Generates a **Folium-based interactive map** with:
   - Plots shown in color.
   - Images marked as dots.
   - Plot-specific popup windows listing matched images.

---

## 💻 How to Run

```bash
python label_by_gps.py
```

Make sure to configure the paths in the script:

```python
input_folder = '/path/to/drone/images/'
geojson_path = '/path/to/plots.geojson'
output_folder = '/path/to/output_folder/'
```

---

## 🔍 Dependencies

Install required packages:

```bash
pip install geopandas shapely folium pillow piexif matplotlib
```

---

## 🖼️ Output Files

- Renamed images saved to:  
  `Plot_labeling_NEW/2025-05-29_Renamed/`

- Interactive HTML map:  
  `Plot_labeling_NEW/2025-05-29_interactive_map.html`

This map shows which images correspond to which plots and helps verify spatial labeling.

---

## ⚠️ Notes

- Skips images with **missing or incomplete EXIF GPS data**.
- The geojson file must contain a field named `Plot_No`.
- Multiple images per plot are allowed.
- The folder date (e.g., `2025-05-29`) is used to timestamp outputs.
- Uses `shapely.distance()` for nearest-plot logic (not just bounding box overlap).

---

## 📌 Why This Matters

Labeling drone images by plot helps:
- Connect field-level measurements with aerial imagery.
- Enable plot-wise segmentation and annotation.
- Prepare datasets for training ML or computer vision models.

---

## 🙌 Acknowledgments

Thanks to open-source packages like `folium`, `geopandas`, and `piexif` for enabling this spatial image labeling tool.

---

*Built for research automation and reproducibility in agricultural field trials and UAV-based phenotyping.*

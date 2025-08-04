# 🏷️ Drone Image Plot Labeling using GPS EXIF Metadata

This repository contains tools to automatically **label drone-captured RGB and thermal images** with the correct **plot number** based on GPS coordinates embedded in the image EXIF data.

## 🎯 Objective

When capturing aerial images over field plots using a drone, each image has associated **GPS EXIF metadata** (latitude, longitude, altitude). These images are taken over agricultural **plots**, each with known boundaries and plot numbers.

The goal is to:
- **Read GPS info** from image EXIF metadata.
- **Compare location** to a geo-referenced `.geojson` file of the field plots.
- **Determine** which plot the image is closest to or overlaps with.
- **Rename the image** by prefixing it with the corresponding plot number.

For example:
```
Original image: IMG_00123.JPG
If closest to Plot 1 → Renamed to: 1_IMG_00123.JPG
```

---

## 📦 Repository Features

- Reads **GPS coordinates** from EXIF metadata using `exifread` or `Pillow`.
- Parses **GeoJSON shapefiles** to extract plot boundaries.
- Uses spatial distance metrics (e.g., centroids, bounding boxes) to match images to the **nearest plot**.
- Handles **incomplete plot sets** (e.g., skips plots 14 and 18).
- Supports **batch processing** of an entire image folder.

---

## 📂 Folder Structure

```plaintext
/Plot_Labeling_Repo/
├── scripts/
│   └── label_images_by_gps.py     # Main labeling script
├── geo/
│   └── field_plots.geojson        # Plot geometry and IDs
├── raw_images/
│   └── IMG_00123.JPG              # Original drone images
├── labeled_images/
│   └── 1_IMG_00123.JPG            # Renamed based on plot match
└── README.md
```

---

## 🧭 How It Works

1. For each image in the `raw_images/` folder:
   - Extracts **latitude/longitude** from EXIF metadata.
2. Loads the **plot boundaries** from the provided `.geojson` file.
3. Uses spatial logic to determine which plot polygon the image is closest to (or intersects with).
4. Renames the image with the matching plot number:
   - `IMG_123.JPG` → `5_IMG_123.JPG` (if matched to Plot 5)
5. Saves the renamed image to `labeled_images/`.

---

## 🧪 Example Use Case

```bash
python scripts/label_images_by_gps.py \
  --input_dir raw_images/ \
  --geojson geo/field_plots.geojson \
  --output_dir labeled_images/
```

---

## ✅ Requirements

Install Python dependencies:

```bash
pip install pillow shapely geopandas exifread
```

Optional: use `pyproj` if projections or coordinate system handling is needed.

---

## 💡 Notes

- Only one image per plot is **not required** — multiple images for the same plot are supported.
- Works best when images are taken **close to nadir (straight down)** for better spatial matching.
- You can modify the script to skip specific plots or apply confidence thresholds on GPS accuracy.
- Make sure `.geojson` polygons are in the same coordinate system as GPS (usually WGS84 / EPSG:4326).

---

## 📍 EXIF Metadata & Matching Logic

Images contain GPS data like:

```
GPSLatitude: 36° 6' 33.43"
GPSLongitude: -115° 9' 58.10"
```

These are converted to decimal degrees and matched against the centroid or bounding box of each plot polygon.

---

## 📬 Contact & Contributions

Feel free to open an issue or pull request if you'd like to contribute improvements or report bugs.

---

🛰️ *This repo bridges raw drone data with structured field experiments by making each image location-aware.*

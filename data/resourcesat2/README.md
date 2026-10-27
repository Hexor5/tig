# Resourcesat-2 (ISRO) image layer

The "Resourcesat-2" map button turns on only after this folder has map tiles
and a `meta.json` file. Until then the button is faded and shows a message.
Nothing is shown from guesswork.

## Step 1 – Download the image (free)
1. Open https://bhoonidhi.nrsc.gov.in and create a free account (login with email/OTP).
2. Search → draw a box around Mohla-Manpur area.
3. Satellite: **Resourcesat-2 / 2A**, Sensor: **LISS-III** (free, 23.5 m). Pick a recent, cloud-free date.
   (LISS-IV 5.8 m is usually paid.)
4. Add to cart → order/download the GeoTIFF (ortho product if available).

## Step 2 – Make map tiles in QGIS (free software)
1. Open the GeoTIFF in QGIS. If it looks dark, set Symbology → Multiband color,
   bands 3-2-1 (or 4-3-2 for false-colour forest), Contrast enhancement: Stretch to MinMax.
2. Processing Toolbox → **Raster tools → Generate XYZ tiles (Directory)**.
   - Extent: the image layer
   - Minimum zoom **8**, Maximum zoom **14**
   - Tile format **PNG** (or JPG for smaller size)
   - "Use inverted tile Y axis (TMS)": **off**
   - Output directory: this folder `data/resourcesat2`
3. QGIS creates folders `8/ 9/ … 14/`. Keep them here.

## Step 3 – Create `meta.json` in this folder
```json
{
  "satellite": "Resourcesat-2",
  "sensor": "LISS-III",
  "scene_date": "YYYY-MM-DD",
  "source": "Bhoonidhi, NRSC/ISRO",
  "bounds": [[SOUTH_LAT, WEST_LON], [NORTH_LAT, EAST_LON]],
  "minzoom": 8,
  "maxzoom": 14,
  "format": "png",
  "tms": false
}
```
Fill `scene_date` and `bounds` from the image (QGIS → layer Properties → Information → Extent,
in EPSG:4326). Set `"format": "jpg"` if you exported JPG, and `"tms": true` only if the
TMS option was on.

## Step 4 – Upload
Upload the whole `data/resourcesat2` folder (with `8/ … 14/` and `meta.json`) to GitHub.
Vercel redeploys; the Resourcesat-2 button becomes active and the map label shows the scene date.

Size tip: zoom 8–14 for one district is usually a few hundred tiles (a few MB) – fine for Vercel.
Check NRSC's data policy and credit "Resourcesat-2 © NRSC/ISRO" (shown automatically on the map).

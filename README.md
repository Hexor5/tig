# Tiger Zone Tracker (CAMPA Forest)

Enter a tiger sighting (latitude, longitude, time). The site shows how far the
tiger could have moved (now, 6 h, 12 h, 24 h, 2 days, 3 days) on an interactive map.
Hindi / English switch at the top right.

Software developed by **Shubham** · hexorhexor66@gmail.com

## Files
| File | Purpose |
|---|---|
| `index.html` | The whole app (design + logic). Map uses Leaflet 1.9.4 from cdnjs. |
| `vercel.json` | Allows browser geolocation, adds security headers. |
| `data/README.md` | How to add a **verified** forest boundary (`data/forest-boundary.geojson`). |

## Features added in this version
- **Leaflet.js** map (pan, pinch-zoom, +/− and "back to tiger" buttons, scale bar).
- **Layers:** Base map (OpenStreetMap), Satellite (Esri), Sentinel-2 (EOX), Terrain (OpenTopoMap);
  overlays: place names, **Forest boundary** (only from verified file), **Bhuvan land use** (optional).
- **Get current location** button (browser Geolocation API, needs HTTPS and user permission).
- **Elevation** field (auto-filled from a DEM; shows "Not available" if the service fails or has no data).
- Coordinate read-out (6 decimals), markers for each sighting, loading indicator, clear error messages.
- If a verified forest boundary is loaded, the result says whether the last sighting is inside or outside it.

## Data services, registration and limits
All services below work **without a credit card or API key**. Attribution is shown on the map automatically.

| Service | Used for | Registration | Limits / rules | Attribution (shown) |
|---|---|---|---|---|
| OpenStreetMap tiles (tile.openstreetmap.org) | Base map | None | [Tile usage policy](https://operations.osmfoundation.org/policies/tiles/): light use only, no bulk download, valid Referer (Vercel default is fine). For heavy traffic use a paid/own tile server. | © OpenStreetMap contributors |
| Esri World Imagery | Satellite | None for viewing tiles | Subject to Esri terms; Esri asks for "Powered by Esri" attribution. For official production use, Esri recommends an ArcGIS Location Platform account (has a free tier) – check current terms. | Powered by Esri, Maxar, Earthstar Geographics |
| Sentinel-2 cloudless 2020 (EOX) | Open satellite (10 m) | None | CC BY-NC-SA 4.0 (non-commercial use, keep attribution). Less detailed than Esri. | Sentinel-2 cloudless by EOX (Copernicus data 2020) |
| OpenTopoMap | Terrain | None | Fair use, CC-BY-SA | © OpenStreetMap, SRTM, OpenTopoMap |
| Esri Reference layers (World_Transportation, World_Boundaries_and_Places) | Roads and place names on satellite | None | Same Esri terms as World Imagery. (CARTO labels were removed: CARTO now requires an API key and stamps "API KEY REQUIRED" on its tiles.) | Esri, HERE, Garmin |
| Esri World Street Map | Automatic fallback if OpenStreetMap tiles fail | None | Same Esri terms | Powered by Esri |
| Bhuvan WMS (NRSC/ISRO) `bhuvan-vec2.nrsc.gov.in/bhuvan/wms` | Land use / land cover 1:50k (optional, off by default) | None for the public WMS | Usage terms for third-party websites are not clearly published. **Get written confirmation from NRSC (Bhuvan) before official launch.** Default layer `lulc:CG_LULC50K_1516` – verify in GetCapabilities; change in Settings. Not a legal forest boundary. | © NRSC/ISRO Bhuvan |
| VEDAS (SAC/ISRO) | **Not integrated** | – | VEDAS terms require ISRO permission to reuse material, so only a link is given. Integrate after written permission. | – |
| Open-Meteo Elevation API | Elevation (Copernicus DEM GLO-90, 90 m) | None for non-commercial | 10,000 calls/day, 5,000/hour, 600/min; CC BY 4.0 | Shown under the field |
| OpenTopoData (SRTM 30 m) | Elevation fallback | None | Public API: 1 call/second, 1,000/day | Shown under the field |
| Google Maps (optional) | Hybrid view | Google Cloud project, **billing account (card) required** | Only appears if you paste your own key in Settings | Google |

Elevation is a DEM estimate (can differ from GPS altitude by several metres). If both
services fail, the field shows "Not available" – no value is invented.

## Deploy on Vercel
**Option A – GitHub (recommended)**
1. Put `index.html`, `vercel.json`, `README.md` and the `data` folder in the root of your GitHub repo (replace the old `index.html`).
2. Commit. Vercel redeploys automatically if the repo is already connected to tig-ten.vercel.app.
3. If not connected: vercel.com → Add New → Project → Import the repo → Framework preset **Other** → no build command → Output directory `./` → Deploy.

**Option B – Vercel CLI**
```bash
npm i -g vercel
cd tiger-site
vercel          # first time: link to existing project "tig-ten"
vercel --prod
```

**After deploy, check on an Android phone (Chrome):**
1. Open https://tig-ten.vercel.app → tap **Get current location** → Allow → latitude, longitude and elevation fill in.
2. Switch Base map / Satellite / Sentinel-2 / Terrain – each should show map images.
3. Turn on **Bhuvan land use** – if the message says the layer didn't load, fix the layer name in Settings.
4. Forest boundary stays "no data" until `data/forest-boundary.geojson` (with metadata) is added.

Geolocation only works on **https://** (Vercel provides this). If permission was denied earlier:
Chrome → ⋮ → Settings → Site settings → Location → tig-ten.vercel.app → Allow.

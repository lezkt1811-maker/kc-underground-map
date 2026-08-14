# KC Underground

An interactive dark map of Kansas City with a scrollable overlay of documented
underground limestone mines (SubTropolis and others), built with [Leaflet](https://leafletjs.com/)
and a plain GeoJSON data file — no build step required.

**[Live demo →](#)** *(update this link once you enable GitHub Pages, see below)*

## What this actually shows

Kansas City sits on the **Bethany Falls Limestone**, a layer that's been mined
by the room-and-pillar method since the late 1800s. A handful of those old
mines were later turned into working underground business parks — the most
famous being **SubTropolis** (Hunt Midwest), a ~55 million sq ft complex
north of the Missouri River.

This repo plots the sites that are actually documented in public sources.
Every feature in `data/mines.geojson` carries a `confidence` field:

| Confidence | Meaning |
|---|---|
| `verified-point` | Coordinates match a published address/source (e.g. SubTropolis) |
| `approximate` | General vicinity is documented, but no public polygon of the mined-out area exists |
| `historical-approximate` | Historic record confirms a site existed; modern georeferencing is a best guess |

**Important honesty note:** there is no single authoritative, publicly
downloadable GIS layer of "every KC-area mine void." Mine operators and
Missouri/Kansas geological surveys hold more precise data than what's on the
open web. The polygon for SubTropolis in this repo is an **illustrative
bounding shape**, not a surveyed footprint — treat it as "mining happens
roughly in this area," not "the tunnels are exactly here."

SubTropolis is north of the Missouri River (Clay County / Kansas City, MO).
Sites in Jackson County (e.g. Independence) are geologically related but are
**separate, unconnected mine systems** — the two don't link up underground.

## Running it locally

No build tools needed. Because the map fetches `data/mines.geojson` via
`fetch()`, you need a local server (opening `index.html` directly via
`file://` will fail due to browser CORS rules):

```bash
cd kc-underground-map
python3 -m http.server 8000
# then open http://localhost:8000
```

## Deploying to GitHub Pages

1. Push this folder to a GitHub repo.
2. Repo **Settings → Pages → Source**: deploy from branch `main`, folder `/ (root)`.
3. Your map will be live at `https://<username>.github.io/<repo>/`.

## Improving the data

The single highest-value upgrade is swapping the hand-placed points/polygons
for real GIS data. Good next sources to pull in:

- **Kansas Geoportal — Quarries and Mines layer**
  `https://hub.kansasgis.org/datasets/KU::quarries-and-mines` (has a public ArcGIS REST endpoint you can query for GeoJSON)
- **Missouri Department of Natural Resources** — Land Reclamation Program / abandoned mine lands records
- **MSHA (Mine Safety and Health Administration)** — mine ID database, `msha.gov`, useful for any site still under active permit
- **USGS Mine Reference / topographic quadrangle maps** showing historic quarry symbols
- County GIS parcel viewers (Jackson County, Clay County, Platte County) sometimes flag known subsidence/mine-void zones

To add a new site, append a `Feature` to `data/mines.geojson` following the
existing shape, and set `confidence` honestly based on what you can actually
source.

## Structure

```
kc-underground-map/
├── index.html          # map + UI, Leaflet loaded from CDN
├── data/
│   └── mines.geojson    # all mapped sites + metadata
└── README.md
```

## Stack

- [Leaflet.js](https://leafletjs.com/) (CDN, no npm install)
- [CARTO Dark Matter](https://carto.com/basemaps) tiles for the base layer
- Esri World Imagery as an optional satellite toggle
- Plain GeoJSON, no backend

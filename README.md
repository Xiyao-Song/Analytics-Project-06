# 🏙️ Manhattan Skyline Explorer

**Explore how Manhattan's skyline has evolved, building by building.**

An interactive geospatial visualization of Manhattan's taller buildings, their construction years, roof heights, and coordinates. Built in **Google Colab** with official NYC Open Data, OpenStreetMap-based tiles, and [deck.gl](https://deck.gl/).

Move through time with a year slider, switch between **3D height columns** and **2D proportional markers**, and explore the tallest buildings on an interactive map. No API key is required.

## Features

- **Interactive construction timeline:** filter buildings completed **by** a selected year or **in** that exact year.
- **Skyline animation:** play through construction history in five-year increments.
- **3D / 2D map:** extruded columns communicate roof height; scalable bubbles offer a top-down alternative.
- **Adjustable height threshold:** focus on buildings between **100 ft and 1,000+ ft** using a minimum-height slider.
- **Height exaggeration:** choose **1×, 2×, or 4×** for easier visual comparison in 3D.
- **Search and inspect:** search by building name or Building Identification Number (BIN); hover over a map marker to see its year, height, latitude, and longitude.
- **Linked rankings:** click a building in the **12 tallest visible buildings** chart to zoom to it, or select a decade to change the timeline.
- **Exports:** save the interactive visualization as HTML, export the complete cleaned data as CSV, or export only the currently filtered buildings directly from the dashboard.

## Quick start — Google Colab

1. Download [`Manhattan_Skyline_Explorer_Colab.ipynb`](./Manhattan_Skyline_Explorer_Colab.ipynb) from this repository.
2. Open [Google Colab](https://colab.research.google.com/), then choose **File → Upload notebook**.
3. Upload the notebook and select **Runtime → Run all** (or run the cells in sequence).
4. Let the notebook retrieve data from NYC Open Data. The interactive dashboard will appear directly in the notebook.
5. Explore the year slider, 3D/2D toggle, height controls, tallest-building ranking, and decade chart.

**Requirements:** An internet connection and a modern web browser. The notebook installs its Python dependencies when needed and does not require a separate API key. If the embedded dashboard does not display in Colab, open the generated `manhattan_skyline_dashboard.html` in your browser.

## How to use the dashboard

| Control | What it does |
| --- | --- |
| **Construction year** | Changes the year displayed on the map. |
| **Built by year / Only in year** | Shows buildings completed on or before the year, or only during that year. |
| **Play skyline growth** | Advances the timeline automatically in five-year steps. |
| **3D columns / 2D bubbles** | Switches the height representation. |
| **Minimum roof height** | Excludes buildings below the chosen height threshold (default **150 ft**). |
| **3D height scaling** | Exaggerates the displayed column heights without changing the underlying measurements. |
| **Search** | Filters by building name or BIN. |
| **12 tallest visible buildings** | Shows the tallest buildings in the active selection; click one to zoom to it. |
| **Completion decade** | Selects a decade to navigate the timeline. |
| **Export selected records · CSV** | Downloads data for the buildings currently visible under your filters. |

**Reading the map:** Each column is anchored at a building's mapped point location. Its height represents the building's recorded **roof height**, converted from feet to meters for the 3D rendering. Column widths are illustrative, **not** building footprints.

## Data source and methodology

**Primary dataset:** [NYC Open Data — BUILDING_P (Building Footprint Centroids)](https://data.cityofnewyork.us/d/u9wf-3gbt)

The notebook requests live records from the NYC Open Data Socrata API, then cleans and filters them before creating the map. The data is **not a hard-coded list of famous skyscrapers**.

To define the initial sample, the notebook:

1. Restricts records to **Manhattan**, using BINs starting with `1`.
2. Keeps mapped buildings with `feature_code = 2100`.
3. Includes records with roof heights of at least **100 ft** and recorded construction years from **1850 through the current year**.
4. Validates coordinates, heights, and years, then removes duplicate point records.
5. Converts roof height from feet to meters using `meters = feet × 0.3048`.

The resulting dataset includes these analysis-ready fields:

| Field | Description |
| --- | --- |
| `id` | Identifier for the mapped building record. |
| `name` | Recorded building name, or a BIN-based fallback label. |
| `bin` | NYC Building Identification Number. |
| `year` | Recorded construction/completion year. |
| `height_ft` | Recorded roof height in feet. |
| `height_m` | Converted roof height in meters. |
| `lat`, `lon` | Latitude and longitude of the mapped building point. |

Other fields (such as `named`) support labeling and display logic.

## Generated outputs

Running the notebook produces:

```text
Manhattan_Skyline_Explorer_Colab.ipynb  # Main notebook
manhattan_skyline_dashboard.html        # Standalone interactive dashboard
manhattan_tall_buildings.csv            # Cleaned building-level dataset
```

The notebook writes the HTML and CSV to its current working directory (normally `/content/` in Colab). To download them, uncomment the `files.download(...)` calls in the final export cell.

The HTML contains the building records and dashboard controls, so it can be shared or hosted independently of the notebook. **An internet connection is still required** to load the online JavaScript library and basemap tiles.

## Tech stack

- **Python**, **pandas**, **requests** — API retrieval, cleaning, and export.
- **NYC Open Data / Socrata API** — building location, year, and roof-height data.
- **deck.gl** — interactive map, 3D columns, 2D markers, and tooltips.
- **CARTO / OpenStreetMap-based basemap** — geographic context.
- **HTML, CSS, JavaScript** — browser-based dashboard and controls.
- **Google Colab / Jupyter** — reproducible notebook workflow.

## Important limitations

- **Not a full historical reconstruction.** Moving the year slider filters **buildings present in today's dataset**; it does not recreate buildings that were demolished or subsequently replaced.
- **Roof height is not total architectural height.** Antennas and spires are not included, so values may differ from published skyscraper rankings.
- **Construction years and names may be incomplete or approximate.** Buildings without usable year, height, or coordinate values are omitted. Some locations are labeled by BIN rather than by a familiar name.
- **The 3D columns are schematic.** They show height at a centroid, not a realistic footprint, façade, or architectural model.
- **Results can change.** The data is fetched when the notebook runs and may be updated by NYC Open Data.

## Data and map attribution

- [NYC Open Data — BUILDING_P](https://data.cityofnewyork.us/d/u9wf-3gbt)
- [NYC Building Footprints metadata](https://github.com/CityOfNewYork/nyc-geo-metadata/blob/main/Metadata/Metadata_BuildingFootprints.md)
- [OpenStreetMap contributors](https://www.openstreetmap.org/copyright)
- [CARTO basemaps](https://carto.com/basemaps)
- [deck.gl](https://deck.gl/)

---

*An exploratory data visualization project for examining Manhattan's building heights, geography, and construction history.*

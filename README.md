# GRID-toolkit
Toolkit to generate square or hexagonal grids and populate them with data 

**© Gabriel M. Ahlfeldt**

Last updated: 11/2025

## General remarks

The **GRID-toolkit** provides a set of Python scripts that allow users to generate square or hexagonal spatial grids centered on user-specified geographic coordinates. Users can define the grid’s **center**, **width**, and **height**, and optionally intersect the generated grid with arbitrary shapefiles to populate its attribute table with spatial data.

The toolkit is designed to work as a modular component within a broader ecosystem of spatial analysis tools, offering **seamless integration** with the following companion toolkits:

- **[AABPL-toolkit](https://github.com/Ahlfeldt/AABPL-toolkit)** — provides global city-level grids with employment and population data.  
- **[MRRH-toolkit](https://github.com/Ahlfeldt/MRRH-toolkit)** — implements general equilibrium spatial counterfactuals based on grid-level data.

---

## Structure

The repository consists of the following main components:

| File/Folder | Description |
|--------------|-------------|
| `GRID-gen.py` | Generates square or hexagonal grids based on user-specified parameters (center coordinates, grid size, resolution, and cell shape). |
| `HEX-gen.py` | Alternative module focusing on hexagonal grid generation. |
| `GRID-data.py` | Extracts and processes data from **AABPL-toolkit** grids and automatically produces output compatible with **MRRH-toolkit**. This includes shapefiles and distance matrices. |
| `input/` | Folder for user-provided shapefiles. As an illustrative example, the toolkit contains the Global Cities grids for San Francisco and San Jose from the **[AABPL-toolkit](https://github.com/Ahlfeldt/AABPL-toolkit)**. They can be replaced with any other input shapefiles.  |
| `output/` | Folder where generated grid shapefiles and derived datasets are saved. |
| `LICENSE` | License information (MIT). |
| `README.md` | Documentation file (this file). |

---

## Key Features

- **Custom grid generation:**  
  Generate square or hexagonal grids by specifying:
  - Geographic coordinates of the grid’s center (latitude, longitude)
  - Grid width and height (in meters or degrees)
  - Cell size and shape (square or hexagon)

- **Spatial data integration:**  
  Intersect generated grids with arbitrary shapefiles to populate grid cells with attribute data.

- **AABPL integration:**  
  Import data from **AABPL-toolkit** global city grids to enrich generated grids with population and employment data.

- **MRRH compatibility:**  
  Automatically generate shapefiles and distance matrices that can be used as direct inputs for **MRRH-toolkit** general equilibrium simulations—without further adjustments.

- **Automated workflow:**  
  Minimal user input required; data extraction, grid creation, and distance matrix computation are streamlined into reproducible processes.

---

## Usage

Python scripts have been developed and tested using **Spyder** and **Anaconda**.

1. **Grid generation:**  
   Define grid parameters (center coordinates, width, height) and run one of the following:
   ```bash
   python GRID-gen.py
   ```
   or
   ```bash
   python HEX-gen.py
   ```

2. **Intersect with data:**  
   Save shapefiles with data from the **AABPL-toolkit** (or other source) in the `input/` folder.  
   Define variables containing population and employment information and the total number of workers in the grid.
   ```bash
   python GRID-data.py
   ```

## GRID-gen.py

### Description

`GRID-gen.py` creates a **regular square grid** (and corresponding centroids) centered on user-defined geographic coordinates.  
It projects geographic coordinates (latitude, longitude) into a **local UTM coordinate system**, constructs grid polygons in meters, and reprojects the result back to **WGS84 (EPSG:4326)** for easy mapping and integration with other spatial data.

The script outputs two shapefiles:
- `grid.shp` — polygon layer of grid cells  
- `centroids.shp` — point layer of grid centroids  

Both files include unique numeric cell IDs and latitude/longitude attributes.

---

### How it works

1. **User settings:**  
   In the **USER SETTINGS BLOCK**, you define grid size and location:
   - `NUM_ROWS` — number of rows in the grid  
   - `NUM_COLS` — number of columns in the grid  
   - `CELL_SIZE_KM` — edge length of each grid cell in kilometers  
   - `CENTROID_LAT` — latitude of the grid center (e.g., `51.5` for London)  
   - `CENTROID_LON` — longitude of the grid center (e.g., `0` for London)  
   - `OUTPUT_FOLDER` — directory where the shapefiles will be saved  

2. **Coordinate transformation:**  
   The script automatically determines the appropriate **UTM projection zone** based on the provided center coordinates.  
   It then converts the center point from geographic (WGS84) to projected (UTM) coordinates in meters.

3. **Grid construction:**  
   Using the number of rows, columns, and cell size, the script computes:
   - The total grid width and height  
   - The upper-left corner of the grid (origin)  
   - The coordinates of each grid cell polygon, generated row-by-row  

4. **Centroid generation:**  
   For each cell, the centroid is calculated as a `Point` geometry.  
   Both polygons and centroids are stored as **GeoDataFrames** (`grid_gdf` and `centroid_gdf`) with the same `cell_id` field for linking.

5. **Reprojection and attribute creation:**  
   Both layers are reprojected back to **EPSG:4326 (WGS84)**.  
   Longitude (`lon`) and latitude (`lat`) attributes are added for convenience.

6. **Export:**  
   The resulting grid and centroid shapefiles are saved to the specified output folder:
   ```
   output/grid.shp
   output/centroids.shp
   ```
   The console confirms successful completion with the grid center coordinates.

---

### What the user should specify

Edit the **USER SETTINGS BLOCK** at the top of the script before running:

```python
# =============================
# USER SETTINGS BLOCK
# =============================
NUM_ROWS = 50             # Number of grid rows
NUM_COLS = 50             # Number of grid columns
CELL_SIZE_KM = 2          # Size of each cell (km)
CENTROID_LAT = 37.558724  # Latitude of grid center
CENTROID_LON = -122.155537  # Longitude of grid center
OUTPUT_FOLDER = "output"  # Folder for output shapefiles
```

- Use coordinates in **decimal degrees (WGS84)**.  
- Adjust `CELL_SIZE_KM`, `NUM_ROWS`, and `NUM_COLS` to define the grid’s total extent.  
- The resulting grid covers approximately  
  ```
  width  = NUM_COLS × CELL_SIZE_KM km  
  height = NUM_ROWS × CELL_SIZE_KM km
  ```
  centered on (`CENTROID_LAT`, `CENTROID_LON`).

---

### Output files

| File | Description |
|------|--------------|
| `grid.shp` | Polygon layer of all grid cells with `cell_id`, `lat`, `lon` attributes. |
| `centroids.shp` | Point layer of grid cell centroids with matching `cell_id`, `lat`, `lon` attributes. |

---

### Notes

- The script installs required packages (`geopandas`, `shapely`, `pyproj`) automatically if missing.  
- The projection choice (UTM zone) ensures accurate distance and area calculations near the grid’s center.  
- Grids can be used directly for intersection with other shapefiles or as inputs for **AABPL-toolkit** and **MRRH-toolkit** workflows.

Example confirmation message after successful execution:
```
OK Grid centered on (37.558724, -122.155537) saved in: output
```

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
| `input/` | Folder for user-provided shapefiles or coordinate specifications. |
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

1. **Prepare inputs:**
   - Define grid parameters (center coordinates, width, height).
   - Optionally, place relevant shapefiles in the `input/` directory.

2. **Run grid generation:**
   ```bash
   python GRID-gen.py

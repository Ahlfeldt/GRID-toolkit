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


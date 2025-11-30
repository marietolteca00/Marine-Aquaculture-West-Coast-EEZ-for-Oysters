# Marine-Aquaculture-West-Coast-EEZ-for-Oysters and Additional Species
- **Author:** Marie Tolteca, Student at Bren School of Environmental Science and Management, Masters in Environmental Data Science
- **Date:** 11/29/2025

##Key concepts:
- Combining vector/raster data, 
- Resampling raster data
- Masking raster data
- Map algebra
- Create a generalizable workflow for additional species

## Data Access
All data can be downloaded via this google drive, provided by Ruth Oliver on EDS 223- Homework Assignment 4- Prioritizing potential aquaculture:
`https://drive.google.com/file/d/1u-iwnPDbe6ZK7wSFVMI-PpCKaRQ3RVmg/view?usp=sharing`

**Data Source:** Historical sea surface temperature (SST) rasters, bathymetry data, and West Coast EEZ shapefiles to identify suitable habitat for marine species aquaculture.

#### Sea Surface Temperature
- National Environmental Satellite Data, and Information Service. Coral Reef Watch. NOAA's 5km Daily Global Satellite Sea Surface Temperature Anomaly v3.1 `https://coralreefwatch.noaa.gov/product/5km/index_5km_ssta.php`. Data Files, SST rasters: `average_annual_sst_2008.tif`,`average_annual_sst_2009.tif`,
`average_annual_sst_2010.tif`, `average_annual_sst_2011.tif` `average_annual_sst_2012.tif`

#### Bathymetry
- General Bathymetric Chart of the Oceans (GEBCO).Depth raster: `depth.tif`.`https://www.gebco.net/data-products/gridded-bathymetry-data#area` 

#### Exclusive Economic Zones (EEZ)
- Maritime boundaries using EEZ polygons off the west coast of the US: `wc_regions_clean.shp`. `https://www.marineregions.org/eez.php`

#### Choosing a New Species
- Find information on species depth and temperature requirements from SeaLife Base for marine aquaculture, a reasonable potential for commerical consumption. `https://www.sealifebase.ca/search.php`

### Required Packages and Setup
```
library(terra)
library(sf)
library(dplyr)
library(tibble)
library(tmap)    
library(here)   
library(knitr)
library(kableExtra)
library(dplyr)
```
## Reprository Set-up
- Add Data folder to .gitignore 
After loading in the data, your reprository should look like this:
<img width="446" height="369" alt="Screenshot 2025-11-29 at 7 07 44 PM" src="https://github.com/user-attachments/assets/75f1487e-cd8b-42be-8d65-1fb2b473fecd" />



# Workflow
- 1. Set up tif's and shapefile to be the same Coordinate Reference System (CRS), extent, resolution, geometry. Before performing spatial analysis, if they do not match, transform them to be aligned to avoid any complications 
- Convert SST rasters from Kelvin to Celsius
- Reproject bathymetry or EEZ polygons if CRS mismatches occur
- Crop and resample depth to match the SST template
This ensures compatibility when stacking, masking, or overlaying layers.

- 2. Identify suitable locations for oysters. Using reclassification matrices convert continuous SST and depth rasters into binary suitability layers.
- SST suitability is determined by oyster thermal ranges
- Depth suitability is determined by viable aquaculture depth ranges
- Multiply reclassified layers to create a combined suitability raster (1 = suitable, 0 = unsuitable)

- 3. Mask suitability to EEZ boundaries and compute area in km.
- Mask the combined suitability raster using the EEZ polygon layer
- Use cellSize() to calculate the real surface area (km²) of each raster cell
- Use zonal() to compute total suitable area (km2) per EEZ region
This summarizes how much area in each West Coast region meets depth and temperature classification.

- 4. Join tables and visualize results
- Join zonal results back to the EEZ spatial object
- Add a calculated percentage column showing:
(suitable area / total EEZ area) × 100
- Create a map showing suitable aquaculture regions along the West Coast using tmap
Final outputs include a suitability map and a table summarizing suitable area per EEZ. 

- 5. Generalizable Workflow:
- In this workflow, the steps 2-4 will be combined into a single function. Step 1 (data preparation: CRS, extent, resolution, geometry alignment) needs to be completed before continuing with function. Once Step 1 is done, the function provides a fully reproducible process for evaluating habitat suitability for any marine aquaculture species within the West Coast EEZ. Simply change the species’ temperature and depth requirements to generate new suitability maps and area summaries.


## Reproducibility

All analysis was conducted in **R studio** using the packages above for visualization. Code is contained in the `aquaculture-wc-eez.qmd` file. 




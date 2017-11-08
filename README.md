# Canada Warbler BCR14 Zonation Project Metadata and Workflow

This project originated from work on the Canada Warbler International Conservation Initiative (CWICI) towards a full life-cycle conservation plan. The preparation of these maps was contributed to in part by Environment and Climate Change Canada (ECCC) with a contract to High Branch Conservation Services. We thank the fifteen conservation, wildlife, and forestry professionals from six regions (provinces and states) who provided their opinions via an expert survey. We also thank the [Boreal Avian Modelling Project](http://www.borealbirds.ca) for data hosting and modelling support.

Reports summarizing the methods and findings of this work can be found at the [project website](http://www.borealbirds.ca/index.php/species-at-risk). This github repository includes data and code only: Please refer to the website for explanations of variables, interpretations, and updates.

_Project collaborators:_

* Dr. Alana Westwood (PI), a.westwood@dal.ca
* Dan Lambert
* Dr. Len Reitsma
* Kara Pearson

## Overview

This process log refers to steps applied using the program Zonation, whose core, user manuals, and supporting documents can be downloaded for free at https://github.com/cbig/zonation-core.

## Data pre-processing

All data layers were assembled using ArcGIS 10.3 in a common geodatabase, [BAM_CAWA2016_BCR14.gdb](CAWA Project 2016\BAM_CAWA2016_BCR14.gdb), (not included in data package).

_Environments for all rasters were set to a common extent (sdm.tif) and snap raster, exported at a resolution of 1 km2 with no data values = -9999. Zeroes were assigned as nodata. ArcGIS tools are indicated using ' '_

| Data layer | Layer description | Layer creation steps | Pre-processing for Zonation | Original data ownership and source |
|----|----|----|----|----|
| [BCR14_mask.tif](/data/BCR14_mask.tif) | Canadian portion of Bird Conservation Region (BCR) 14 | Clipped the international BCR14 polygon layer to the extent of BCR14, then to the boundaries of Canada. 2 km buffer was added to the perimeter to account for cross-border data (if available) and discrepancies with georeferencing alignment between datasets. Field code added and all land mass coded as 1 | 'Feature to raster' | [North American Bird Conservation Initiative](http://www.nabci.net/Canada/English/bird_conservation_regions.html) |
| [protected.tif](/data/protected.tif) | National, provincial, and privately protected areas ranked as IUCN category I-IV recorded in the CARTS Conservation Areas Reporting and Tracking System (CARTS) geodatabase | Added field to attribute table and coded all protected areas as "1" | 'Feature to raster' | [CARTS 2015](http://www.ccea.org/carts) |
| [tenures.tif](/data/tenures.tif) | Extent of areas owned or leased (on Crown land) by forestry companies for industrial harvesting as identified by Global Forest Watch (GFW) | Isolated forestry tenures by category and merged. Added attribute field and coded all forestry tenures polygons = 1 | 'Feature to raster' |  [Global Forest Watch Canada](https://www.globalforestwatch.ca/node/273) |
| [ssi_CAWA2005.txt](/data/ssi_CAWA2005.txt) | Known locations of Canada Warblers (CAWA) 2005-2009 | On the BAM dataset of CAWA presence locations, used 'select by attribute' (all points 2005-2009) and on this set, 'select by location' (all points in BCR14). Exported as new layer and merged all points. Calculated new X and Y values and deleted points with identical values.  Added field and coded as 1. | 'Feature to raster' | [BAM](www.borealbirds.ca) |
| [ssi_CAWA2010.txt](/data/ssi_CAWA2010.txt) | Known locations of Canada Warblers (CAWA) 2010-2015 | On the BAM dataset of CAWA presence locations, used 'select by attribute' (all points 2010-2015) and on this set, 'select by location' (all points in BCR14). Exported as new layer and merged all points. Calculated new X and Y values and deleted points with identical values. Added field and coded as 1. |  'Feature to raster' | [BAM](www.borealbirds.ca) |
| [clm_current.tif](/data/clm_current.tif) | Species distribution model (SDM) of mean projected population density of Canada Warbler based on climate variables in 2014 by Stralberg et al. (2014) | 'Extract by mask' to clip SDM to BCR14 Extent. 'Reclassify'used to rescale values from 1-100 | 'Feature to raster' | [BAM](www.borealbirds.ca) |
| [clm_2050.tif](/data/clm_2050.tif) | Species distribution model (SDM) of mean projected population density of Canada Warbler based on climate variables in 2050 by Stralberg et al. (2014) | 'Extract by mask' to clip SDM to BCR14 Extent. 'Reclassify'used to rescale values from 1-100 | Raster data exported | [BAM](www.borealbirds.ca) |
| [clm_2080.tif](/data/clm_2080.tif) | Species distribution model (SDM) of mean projected population density of Canada Warbler based on climate variables in 2080 by Stralberg et al. (2014) | 'Extract by mask' to clip SDM to BCR14 Extent. 'Reclassify'used to rescale values from 1-100 | Raster data exported | [BAM](www.borealbirds.ca) |
| [sdm.tif](/data/sdm.tif) | Median CAWA population density modelled by Haché et al. (in review) | Clipped SDM to extent of BCR14 | When exporting to raster, set to extent of BCR14 Canada but added one row of nodata to all 4 sides of raster square | [BAM](www.borealbirds.ca) |
| [uncertainty.tif](/data/uncertainty.tif) | Standard deviation of CAWA population density modelled by Haché et al. (in review) | Clipped SDM to extent of BCR14 | Raster data exported | [BAM](www.borealbirds.ca) |
| [footprint.tif](/data/footprint.tif) | A composite measure of the extent and relative intensity of human influence on terrestrial ecosystems by Woolmer et al. (2008) | 'Extract by mask' to BCR 14. Inverted and recoded between 0 and 1 using 'raster calculator' formula 1.0 - ("footprint" / 100.0) | Raster data exported | [The Nature Conservancy - Eastern Conservation Science](https://2c1forest.databasin.org/maps/134fcdd80c454a8f9f49c648a4b9894d) |
| [uplands.tif](/data/uplands.tif) | Uplands as defined by the Northeastern Habitat Types (Anderson et al. 2013) common classification for ecosystems and habitats | 'Extract by attributes' for class 'wetlands', 'Reclassify' with formula uplands = 1, 'Extract by mask' to extent of BCR 14 | Raster data exported | [The Nature Conservancy - Eastern Conservation Science](https://www.conservationgateway.org/ConservationByGeography/NorthAmerica/UnitedStates/edc/reportsdata/terrestrial/habitatmap/Pages/default.aspx) |
| [wetlands.tif](/data/wetlands.tif) | Wetlands as defined by the Northeastern Habitat Types (Anderson et al. 2013) common classification for ecosystems and habitats | 'Extract by attributes' for class 'wetlands', 'Reclassify' with formula wetlands = 1, 'Extract by mask' to extent of BCR 14 | Raster data exported | [The Nature Conservancy - Eastern Conservation Science](https://www.conservationgateway.org/ConservationByGeography/NorthAmerica/UnitedStates/edc/reportsdata/terrestrial/habitatmap/Pages/default.aspx) |




## Analysis
_Steps completed in [Zonation 4.0](https://github.com/cbig/zonation-core)_.


1. Features

Hierarchical mask - could code areas already protected (2), areas available for protection (1), and areas unavailable for protection (0)

### Variants
_Intermediate analyses, adding components iteratively to catch errors_

1. i_caz_SDM
1. ii_cazSDM_cnd
1. iii_cazSDM_cnd_dbs
1. iv_cazSDM_cnd_dbs_ssi
1. v_cazSDM_cnd_dbs_ssi_unc
1. C1_gcm_mtx
1. C2_gcm_mtx_clm
1. C2_gcm_mtx_clm2
1. M1_gcm_adm
1. M2_gcm_adm_mtx
1. M2_gcm_adm_mtx2

Codes in the names are interpreted as follows:

| Code | Description                                  |
|------|----------------------------------------------|
| caz  | Cell removal rule: core-area zonation |
| sdm  | Input features: Species distribution model                   |
| cnd  | Condition layer                      |
| dbs  | Distribution smoothing                             |
| ssi  | Input features: species of special interest                             |
| unc | Uncertainty discounting |
| gcm | General conservation model |
| mtx | Connectivity: matrix connectivity |
| clm | Connectivity: interaction connectivity (climate change) |
| adm | Administrative units |

### Variants by scenario

![Scenarios for CAWA modelling](https://github.com/borealbirds/cawa-bcr-14/raw/master/BCR14%20CAWA%20spatial%20model%20flowchart.jpg)

#### General conservation model development

*i_01_cazSDM* - CAWA species distribution model with core area zonation
*ii_cazSDM_cnd* - CAWA SDM (CAZ) + condition layer
*iii_cazSDM_cnd_dbs* - SDM (CAZ) + condition + distribution smoothing
*iv_cazSDM_cnd_dbs_ssi* - SDM (CAZ) + condition + distribution smoothing + species of special interest (ssi)
*v_cazSDM_cnd_dbs_ssi_unc* - SDM (CAZ) + condition + distribution smoothing + ssi + uncertainty. This model = general conservation model (GCM)

#### Class 1: Long term conservation

*C1_1_gcm_mtx* - GCM + matrix connectivity to protected areas
*C1_2_gcm_clm* - GCM + interaction connectivity (climate 2050)
*C1_2_gcm_clm* - GCM + interaction connectivity (climate 2080)

#### Class 2: Habitat management

*C2_1_gcm_adm* - GCM + administrative regions
*C2a_gcm_adm_mtx* - GCM + administrative regions + matrix connectivity to wetlands
*C2a_gcm_adm_mtx* - GCM + administrative regions + matrix connectivity to uplands

### Run settings and notes by variant

Follow links for all run and output files.
#### i. [Species Only](/i_caz_SDM/)
Input feature: [sdm.tif](/data/sdm.tif)
Mask: [BCR14_mask.tif](/data/bcr14_mask.tif)
Notes:
- Did a preliminary comparison of additive benefit features (ABF) and CAZ, and with with CAZ used a dispersal kernel (α) added into species feature, calculated by α = 2/dispersal distance in map units = 2/5000 = 0.0004
- because there is only one feature layer, the results are identical
- CAZ was retained

#### ii. [Disturbance Discounting](/ii_cazSDM_cnd/)
Input feature: [sdm.tif](/data/sdm.tif)
Mask: [BCR14_mask.tif](/data/bcr14_mask.tif)
Discounting feature: [footprint.tif](/data/footprint.tif)
Notes:
- none

#### iii. [Connectivity](/iii_cazSDM_cnd_dbs/)
Input feature: [sdm.tif](/data/sdm.tif)
Mask: [BCR14_mask.tif](/data/bcr14_mask.tif)
Discounting feature: [footprint.tif](/data/footprint.tif)

Notes:
- Added distribution smoothing using the dispersal kernel (5 km) to aggregate areas
- Preliminary comparison to boundary length penalty, which on visual inspection caused removal of too many small areas of high value. - Chose dispersal kernal for analysis moving forward


#### iv. [Species of Special Interest](/iv_cazSDM_cnd_dbs_ssi/)
Input features: [sdm.tif](/data/sdm.tif), [ssi_CAWA2005.txt](/data/ssi_CAWA2005.txt), [ssi_CAWA2010.txt](/data/ssi_CAWA2010.txt)
Mask: [BCR14_mask.tif](/data/bcr14_mask.tif)
Discounting feature: [footprint.tif](/data/footprint.tif)

Notes:
- added locations of CAWA from 2005-2009 and  as well as 2010-2015.
- As connectivity analyses do not function on these points, it means there are 1x1 km areas retained as high value where CAWA have been found to occur.
- Chose to retain this method to ensure repeatedly used habitats are retained in final models

#### v. [Uncertainty](/v_cazSDM_cnd_dbs_ssi_unc/)
Input features: [sdm.tif](/data/sdm.tif), [ssi_CAWA2005.txt](/data/ssi_CAWA2005.txt), [ssi_CAWA2010.txt](/data/ssi_CAWA2010.txt)
Mask: [BCR14_mask.tif](/data/bcr14_mask.tif)
Discounting features: [footprint.tif](/data/footprint.tif), [uncertainty.tif](/data/uncertainty.tif)

Notes:
- none

#### C1. [Connectivity to existing protected areas](/C1_gcm_mtx/)
Input features: [sdm.tif](/data/sdm.tif), [ssi_CAWA2005.txt](/data/ssi_CAWA2005.txt), [ssi_CAWA2010.txt](/data/ssi_CAWA2010.txt), [protected.tif](data/protected.tif)
Mask: [BCR14_mask.tif](/data/bcr14_mask.tif)
Discounting features: [footprint.tif](/data/footprint.tif), [uncertainty.tif](/data/uncertainty.tif)

Notes:
- This model was first attempted by adding a hierarchical mask of protected areas, but the result screened these areas out as the most desirable rather than supporting connectivity to them
- Instead added protected areas  as a feature layer and used a connectivity matrix, increasing the value of CAWA areas within 5 km of protected areas by 0.1.
- We experimented with higher weights (0.5, 0.2), but these emphasized the solution too strongly towards protected areas based on visual analysis

#### C2. [Medium-term climate change (2050)](/C1_gcm_mtx_clm/)
Input features: [sdm.tif](/data/sdm.tif), [ssi_CAWA2005.txt](/data/ssi_CAWA2005.txt), [ssi_CAWA2010.txt](/data/ssi_CAWA2010.txt), [protected.tif](data/protected.tif), [clm_current.tif](/data/clm_current.tif), [clm_2050.tif](/data/clm_2050.tif)
Mask: [BCR14_mask.tif](/data/bcr14_mask.tif)
Discounting features: [footprint.tif](/data/footprint.tif), [uncertainty.tif](/data/uncertainty.tif)

Notes:
- set weight of climate SDM layers to 0.75 and added ecological interactions between current and future climate projection
- beta value (connectivity) was set at 5km x 36 years, so 180 km, 2/180 000 m = 0.000011

#### C3. [Long-term climate change (2080)](/C1_gcm_mtx_clm/)
Input features: [sdm.tif](/data/sdm.tif), [ssi_CAWA2005.txt](/data/ssi_CAWA2005.txt), [ssi_CAWA2010.txt](/data/ssi_CAWA2010.txt), [protected.tif](data/protected.tif), [clm_current.tif](/data/clm_current.tif), [clm_2080.tif](/data/clm_2050.tif)
Mask: [BCR14_mask.tif](/data/bcr14_mask.tif)
Discounting features: [footprint.tif](/data/footprint.tif), [uncertainty.tif](/data/uncertainty.tif)
Notes:
- set weight of climate SDM layers to 0.75 and added ecological interactions between current and future climate projection
- beta value (connectivity) was set at 5km x 66 years, so 330 km, 2/330 000 = 0.000006

#### M1 [Focus on forestry tenures](/M1_gcm_adm/)
Input features: [sdm.tif](/data/sdm.tif), [ssi_CAWA2005.txt](/data/ssi_CAWA2005.txt), [ssi_CAWA2010.txt](/data/ssi_CAWA2010.txt), [protected.tif](data/protected.tif),
Mask: [BCR14_mask.tif](/data/bcr14_mask.tif), [tenures.tif](/data/tenures.tif)
Discounting features: [footprint.tif](/data/footprint.tif), [uncertainty.tif](/data/uncertainty.tif)
Notes:
- forced selection of pixels into administrative units based on forestry tenures, where high-value areas are aggregated for strong local representation
- set forestry tenures as administrative units with a weight of 0.5 after visual experimentation with higher and lower weights
- no connectivity to protected areas

#### M2 [Retention areas](/M2_gcm_adm_mtx/)
Input features: [sdm.tif](/data/sdm.tif), [ssi_CAWA2005.txt](/data/ssi_CAWA2005.txt), [ssi_CAWA2010.txt](/data/ssi_CAWA2010.txt), [protected.tif](data/protected.tif), [wetlands.tif](data/wetlands.tif)
Mask: [BCR14_mask.tif](/data/bcr14_mask.tif), [tenures.tif](/data/tenures.tif)
Discounting features: [footprint.tif](/data/footprint.tif), [uncertainty.tif](/data/uncertainty.tif)
Notes:
- added matrix connectivity to protected areas network with a weight of 0.5
- included matrix connectivity to as wet-poor habitats within forestry tenures with a weight of 0.5

#### M3 [Managing for future habitat](/M3_gcm_adm_mtx2/)
Input features: [sdm.tif](/data/sdm.tif),   [wetlands.tif](data/wetlands.tif), [protected.tif](data/protected.tif), [uplands.tif](data/wetlands.tif)
Mask: [BCR14_mask.tif](/data/bcr14_mask.tif), [tenures.tif](/data/tenures.tif)
Discounting features: [uncertainty.tif](/data/uncertainty.tif)
Notes:
- Retained connectivity with wetlands and protected areas, but weighted them negatively (-0.5)
- Uplands was added with no feature weight but a positive impact on connectivity (+0.5)
- removed SSI and anthropogenic disturbance

## Post-Processing/Post-hoc Analysis
*In progress. Please check back for updates.*

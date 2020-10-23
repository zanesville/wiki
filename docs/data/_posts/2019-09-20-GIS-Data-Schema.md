---
title: GIS Data Schema
tags: postgis
subtitle: The how and why of the layer naming conventions in the GIS Database
---
## Metadata
Postgres allows for the editing of a "Comments" field in the table metadata. This can be used to store metadata on the layer, as there is no other built-in method for storing metadata in Postgres, and the metadata fields in QGIS exist only inside the current project. This Comments field will show up in QGIS under Properties-Information-Comments.

## Naming Convention
This post outlines the naming convention for the layers in the GIS database. I also include some reference material and how I came up with the naming scheme.

Typical layer name:

```sql
utl_stormwater_impervious_areas /*category_ownerIfNotZanesville_division_OR_goup_layer_projection (if not 3735)*/
```

Layer names should be all lowercase, with no spaces, as with all field names (Postgres convention). The naming convention is as follows, with definitions of each section of the name found below.

>``category_ownerIfNotZanesville_division_OR_goup_layer_projection (if not 3735)``

Projections are 3735 NAD83 SP Ohio South unless otherwise noted in the layer name. *Web map data if on Mapbox GL JS or AGOL is in Web Mercator and is transformed with the IRTF00 transformation during the export process.*  The layer content should be easily determined by the layer name, so abbreviations are used sparingly. Data years can be omitted for most layers as updating these would break source links when updated. The year or publication should be included in the attributes or metadata for the layer. Older versions of the data, if kept, should include the year or have the date in the layer name. One exception is imagery; imagery should always have the year in the layer name. If the data owner is not the City of Zanesville, the data owner should be in the name. Owners should be easily identifiable if abbreviated, such as “mus” for Muskingum County. Common abbrevations include:

* odot
* mus
* odnr
* fema
* census
* esri (Esri data from the downloadable datasets should not be hosted online unless the Terms of Service for that layer explicitly allows publishing)

**For administrative, engineering and a few other layers, look under all the group types as the delination between what group things should be in was my own choice and may not be immediately apparent.** For example, right of way could go under administrative, engineering or transportation. I put the layer under engineering but it would fit into any of those categories.

Datasets created for one-off public request maps should go in their repsective folder - Z:/scans/GIS_Projects/Year/Folder Name

---
## Map Layer Groups 

* Administraive Boundaries - adm
* Census/Demographics -  cen
* Utilities -  utl - utl_stormwater, utl_wastewater_stm, utl_wastewater_san
* Community Development & Zoning -  dev
* Environmental - env
* Parks & Recreation/Cemeteries - rec_parks, rec_cemeteries
* Air/Roads/Rail/ -  trn
* Public Safety/Police/Fire/Code - psf_fire, psf_police, psf_code (NOT IN USE)
* Engineering - eng
* Imagery - img
* Streets & Sanitation - utl_sanitation, utl_streets

## Database
We only have one database.  ArcGIS software does not allow for use of the ``postgres`` default database.

> TODO - CONVERT LAYER LIST TO ACCORDIANS WITH JAVASCRIPT

### Example

Layer | Name | Category |
--- | --- | --- |
<strong>Engineering</strong> | | 
 eng_mus_eop | Test | Test
 eng_mus_row | Test | Test

*For all layers see the database in Postgres.*

---

## Reference

### Mapzen Map Style Layer Groups
boundaries, buildings, earth, landuse, places, pois, roads, transit, and water

### Example from GeoNet
> So for example, i18_CalTrans_MjrHwy_15, where the i18 (transportation ISO), CalTrans (author of data), MjrHwy (subject), 15 (year of publication)

---
title: GIS Data Schema
tags: postgis
subtitle: The how and why of the layer naming conventions in the GIS Database
---

## Naming Convention
This post outlines the naming convention for the layers in the GIS database. I also include some reference material and how I came up with the naming scheme.

Typical layer name:

```sql
utl_stormwater_impervious_areas /*category_ownerIfNotZanesville_division_OR_goup_layer_projection (if not 3735)*/
```

Layer names should be all lowercase, with no spaces, as with all field names (Postgres convention). The naming convention is as follows, with definitions of each section of the name found below.
>``category_ownerIfNotZanesville_division_OR_goup_layer_projection (if not 3735)``

Projections are 3735 NAD83 SP Ohio South unless otherwise noted in the layer name. *Web map data if on Mapbox GL JS or AGOL is in Web Mercator and is transformed with the IRTF00 transformation during the export process.*  The layer content should be easily determined by the layer name, so abbreviations are used sparingly. Data years can be omitted for most layers as updating these would break source links when updated. The year or publication should be included in the attributes or metadata for the layer 🤣. Older versions of the data, if kept, should include the year or have the date in the layer name. One exception is imagery; imagery should always have the year in the layer name. If the data owner is not the City of Zanesville, the data owner should be in the name. Owners should be easily identifiable if abbreviated, such as “mus” for Muskingum County. Common abbrevations include:

* odot
* mus
* odnr
* fema
* census
* esri (Esri data from the downloadable datasets should not be hosted online unless the Terms of Service for that layer explicitly allows publishing)

**For administrative, engineering and a few other layers, look under all the group types as the delination between what group things should be in was my own choice and may not be immediately apparent.** For example, right of way could go under administrative, engineering or transportation. I put the layer under engineering but it would fit into any of those categories.

Datasets created for one-off public request maps should be named request_yyyymmdd_title and stored under GIS_Public_Requests/Year/Folder of the PR as shapefiles or GeoPackages

---
## Map Layer Groups 

* Administraive Boundaries - adm
* Census/Demographics -  cen
* Utilities -  utl - utl_stormwater, utl_wastewater_stm, utl_wastewater_san
* Community Development & Zoning -  dev
* Environmental - enviro - env
* Parks & Recreation/Cemeteries - rec_parks, rec_cemeteries
* Air/Roads/Rail/ -  trn
* Public Safety/Police/Fire/Code - psf_fire, psf_police, psf_code
* Places - poi
* Engineering - eng
* Imagery - img
* Streets & Sanitation - utl_sanitation, utl_streets

## Database Names
We only have one database - ``coz``.  ArcGIS software does not allow for use of the ``postgres`` default database.

> TODO - CONVERT LAYER LIST TO ACCORDIANS WITH JAVASCRIPT

Layer | Name | Category |
--- | --- | --- |
<strong>Engineering</strong> | | 
 eng_mus_eop | Test | Test
 eng_mus_row | Test | Test
 eng_building_footprints | Test | Test
 eng_culverts | Test | Test
 eng_centerlines_pci | Test | Test
 eng_roads_signs | Test | Test
 eng_streetview_points | Test | Test
 eng_streetview_photos | Test | Test
 eng_projects (geocoded to points or polygons) | Test | Test
 eng_private_development (geocoded to points or polygons) | Test | Test
 eng_permits_stormwater_residential | Test | Test
 eng_permits_stormwater_commercial | Test | Test
 eng_permits_stormwater_commercial | Test | Test
 eng_permits_curbcut | Test | Test
 eng_permits_row | Test | Test
 eng_places_of_interest - NOT SURE ABOUT THIS ONE COULD GO UNDER ADMIN OR PARKS AND REC	 | Test | Test


## Web Map Layers

The following layers are available in the geojson and vector tile cache. A few layers may exists in the database and not in the cache, for example due to being too large to export as geojson and convert.

<pre id="js-layers"></pre>

* transportation 
  * trn_mus_centerlines
  * trn_esri_rail
  * trn_odot_bridges

* engineering
  * eng_mus_eop
  * eng_mus_row
  * eng_building_footprints
  * eng_culverts
  * eng_centerlines_pci
  * eng_roads_signs
  * eng_streetview_points
  * eng_streetview_photos
  * eng_projects (geocoded to points or polygons)
  * eng_private_development (geocoded to points or polygons)
  * eng_permits_stormwater_residential
  * eng_permits_stormwater_commercial
  * eng_permits_stormwater_commercial
  * eng_permits_curbcut
  * eng_permits_row
  * eng_places_of_interest - NOT SURE ABOUT THIS ONE COULD GO UNDER ADMIN OR PARKS AND REC	
 
* admin
  * adm_mus_administrative_boundaries
  * adm_mus_parcels
  * adm_mus_addresses
  * adm_mus_annexations
  * adm_mus_voting_precincts

  * schools
    * adm_school_districts
    * adm_school_building_points (could be a subset of pois)
    * adm_school_building_footprints

* utilities
  * sewers
    * utl_sanitary_lines
    * utl_sanitary_points
    * utl_sanitary_photos
    * utl_sanitary_plans - lines or polygons
    * utl_stormwater_mains
    * utl_stormwater_points
    * utl_stormater_photos
    * utl_stormater_plans
    * utl_mus_sanitary_lines
    * utl_mus_sanitary_points

  * water
    * utl_water_lines
    * utl_water_points

  * electric
    * utl_elec_conduit
    * utl_elec_utility_poles
    * utl_elec_signal_poles
    * utl_elec_light_poles
    * utl_elec_pedestrian_signals

  * stormwater utility
    * utl_stormwater_impervious_areas
    * utl_stormwater_impervious_areas_dissolve
    * utl_stormwater_billing_table
    * utl_stormwater_credits_table
    * utl_stormwater_plans (linked to polygons)
    * utl_stormwater_permits (points)

  * gas
    * utl_ukn_gas_lines

  * electric
    * utl_ukn_electric_lines
    * utl_ukn_electric_points
    * utl_ukn_electric_service_areas
    * utl_ukn_electric_poles

* parks & recreation
  * cemeteries
    * rec_cemetery_boundaries
    * rec_cemetery_plot_boundaries
    * rec_cemetery_plans
    * rec_cemetery_plot_points
    * rec_cemetery_photos
  * parks
    * rec_park_boundaries
    * rec_park_points
    * rec_park_trails
    * rec_park_poi
    * rec_park_tree_inventory


* environmental
  * env_fema_nfhl
  * env_usgs_hyrdro
  * env_mus_contours
  * env_odnr_wetlands

* public safety
  * fire
    * psf_fire_districts
    * psf_fire_stations
    * psf_fire_hydrants
 
* community development
  * dev_drd_boundaries
  * dev_econ_dev_districts
  * dev_brownfield_boundaries 
  * dev_zoning
  * dev_zoning_districts
  * dev_zoning_boundaries
  * dev_zoning_boundaries_dissolve
  * dev_zoning_wards

---

## Reference

### Mapzen Map Style Layer Groups
boundaries, buildings, earth, landuse, places, pois, roads, transit, and water

### Example from GeoNet
> So for example, i18_CalTrans_MjrHwy_15, where the i18 (transportation ISO), CalTrans (author of data), MjrHwy (subject), 15 (year of publication)

<script>

fetch('https://311.coz.org/data/dataIndex.json')
.then(res => { return res.json()})
.then(data => {
	var div = document.querySelector("#js-layers");
	
	var length = data.length;
	var num = document.createElement("div");
	num.innerText = "Total layers available in the cache " + length;
	div.insertBefore(num, null);
	div.innerText = JSON.stringify(data,0,2)
})

var table = document.querySelector("table");
table.className = "table";
</script>
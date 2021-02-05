---
title: Updating the Tax Parcels
tags: arcgispro webmaps
subtitle: Step by Step Process for Updating Parcel Data from the County GIS
category: tasks
---

The parcel layer is created from combining the parcel shapefile from the county and several sheets from the county tax data. Several web map layers, materialized views, and AGOL applications depend on this parcel layer in Postgres (the AGOL app needs the parcel layer uploaded to AGOL). This means that once a parcel update layer is created (in a FileGeodatabase), the Postgres data cannot be simply overwritten (as this does a DELETE and CREATE). The Postgres table is backed up, truncated (all rows deleted) and then using the parcel update, appended with the parcel upate table.

> Updating the parcel file seems like a straightforward task, but the process can sometimes take several hours depending on whether or not there are new files from the county, new data schemas, problems with the Postgres import, problems with the AGOL updload...etc.

## Create the Parcel Update
All raw files for the update are located in the GIS\Tax Parcels\ParcelMUS\Updates folder

1. Download ExtractExcel to Z:\scans\GIS\Tax Parcel\ParcelsMUS\Updates
  - ftp://ftp.mceo.org/Transfer/GIS/Tax%20Data/
2. Download Full Export (also named Extract Excel) to Z:\scans\GIS\Tax Parcel\ParcelsMUS\Updates
  - ftp://ftp.mceo.org/Transfer/GIS/Tax%20Data/ to ExportExcelFull.zip
4. Extract both files to their folders, overwriting the existing files.
5. Download the latest parcel file from DDTI [https://downloads.ddti.net/muskingumoh/](https://downloads.ddti.net/muskingumoh/)
6. Overwrite the existing Parcel.shp with this new file
7. Run the Parcels Update model from the ArcPRO TaxParcelUpdates Project in the GIS\Tax Parcels folder.

## Apply the Update to the Parcel Table in Postgres vis pg_admin & QGIS
Running this during working hours could affect the web maps, better to do it first thing in the morning.

1. Load the parcel file into QGIS using the Add Vector Layer/Directory/OpenFileGDB - using the layer in the FGDB "adm_mus_parcels"
2. Run Fix Geometries in QGIS on the updated parcel file.
3. Add the field coz_update to the fixed geometries
4. Calculate the field coz_update using ``now()`` to get the current date
5. Import the fixed geometry parcels into Postgres as ``adm_mus_parcels_update``, overwriting existing and converting all fields to lowercase using the Export to Postgres tool.
6. DO NOT OVERWRITE THE EXISTING adm_mus_parcels LAYER
7. Run the following SQL commands in QGIS or pg_admin below in sequence to update the existing parcel data from the new updated table.
8. Once the table has been upated, refresh the materialized Views that rely on parcels - zoning view, ward lookup view, and geocoder view - in that order.

```SQL
/*backup current parcels*/
CREATE TABLE adm_mus_parcels_bak as TABLE adm_mus_parcels

/*delete all rows in main parcel table and restart the ID Identity sequence*/
/*Note that this means you should NEVER join anything to the parcel table using the id field*/
TRUNCATE TABLE adm_mus_parcels
RESTART IDENTITY

/*insert all rows from the updated table into the existing table*/
INSERT INTO adm_mus_parcels SELECT * FROM adm_mus_parcels_update
```

## Update the Public Notification Web Layer
1.Update the Public Notification AGOL Web Map Layer from ArcGIS Pro by overwirting existing service. The ArcPro layers read from Postgres.
 1. ``\scans\GIS_Projects\Web Applications\Production\commdev-public-notification-web-application\public-notification-web-application.aprx``
2. Share > Web Layer >Overwrite Web Layer 

**NOT REPLACE WEB LAYER**
 
## Final Step
 
If everything went well, delete the ``adm_mus_parcels_update`` table in Postgres.
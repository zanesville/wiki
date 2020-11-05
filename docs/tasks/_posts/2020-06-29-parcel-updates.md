---
title: Updating the Tax Parcels
tags: arcgispro webmaps
subtitle: Step by Step Process for Updating Parcel Data from the County GIS
---

The parcel layer is created from combining the parcel shapefile from the county and several sheets from the county tax data. Several web map layers are materialized views that depend on this parcel layer in Postgres. This means that once a parcel update layer is created (in a FileGeodatabase), the Postgres data cannot be simply overwritten (as this does a DELETE and CREATE). The Postgres table is backed up, truncated (all rows deleted) and then using the parcel update, appended with the parcel upate table.

> Updating the parcel file seems like a straightforward task, but the process can sometimes take several hours depending on whether or not there are new files from the county, new data schemas, problems with the Postgres import, problems with the AGOL updload...etc.

## Create the Parcel Update
1. Download ExtractExcel to Z:\scans\GIS\Tax Parcel\ParcelsMUS\Updates
  - ftp://ftp.mceo.org/Transfer/GIS/Tax%20Data/
2. Download Full Export ((also named Extract Excel) to Z:\scans\GIS\Tax Parcel\ParcelsMUS\Updates
  - ftp://ftp.mceo.org/Transfer/GIS/Tax%20Data/
3. Rename Full Export folder to ExtractExcelFull.
4. Extract both files to their folders, overwriting the existing files.
5. Download the last updated TAXPARCEL**.shp or latest parcel file (check the modified date in the ftp) and overwrite existing.
  5. ftp://ftp.mceo.org/Transfer/GIS/Parcel/
6. Rename to TAXPARCEL19 so that it will work in the model.
7. Run the Parcels Update model from the ArcPRO TaxParcelUpdates Project in the GIS\Tax Parcels folder.

## Apply the Update to the Parcel Table in Postgres

1.Run Fix Geometries in QGIS on the updated parcel file.
2.Import the parcels into Postgres as ``adm_mus_parcels_update``, overwriting existing and converting all fields to lowercase.
    2. DO NOT OVERWRITE THE EXISTING adm_mus_parcels LAYER
3. Run the following SQL commands below in sequence to update the existing parcel data from the new updated table.
4.Once the table has been upated, refresh the materialized Views that rely on parcels - zoning view, ward lookup view, and geocoder view - starting with the zoning view.

```SQL
/*backup current parcels*/
CREATE TABLE adm_mus_parcels_bak as TABLE adm_mus_parcels

/*delete all rows in main parcel table and restart the ID Identity sequence*/
TRUNCATE TABLE adm_mus_parcels
RESTART IDENTITY

/*insert all rows from the updated table into the existing table*/
INSERT INTO adm_mus_parcels SELECT * FROM adm_mus_parcels_update
```

## Update the Public Notification Web Layer
1.Update the Public Notification AGOL Web Map Layer from ArcGIS Pro by overwirting existing service - reads from Postgres.
 1. ``\scans\GIS_Projects\Web Apps Production\commdev-public-notification-web-application\public-notification-web-application.aprx``
 
 ## Final Step
 
 If everything went well, delete the ``adm_mus_parcels_update`` table in Postgres.


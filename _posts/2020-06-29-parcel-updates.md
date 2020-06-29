---
title: Updating the Tax Parcels
tags: arcgispro webmaps
subtitle: Step by Step Process for Updating Parcel Data from the County GIS
---

> Updating the parcel file seems like a straightforward task, but the process can sometimes take several hours depending on whether or not there are new files from the county, new data schemas, problems with the Postgres import, problems with the AGOL updload...etc.

1. Download ExtractExcel to Z:\scans\GIS\Tax Parcel\ParcelsMUS\Updates
 - ftp://ftp.mceo.org/Transfer/GIS/Tax%20Data/

2. Download Full Export ((also named Extract Excel) to Z:\scans\GIS\Tax Parcel\ParcelsMUS\Updates
 - ftp://ftp.mceo.org/Transfer/GIS/Tax%20Data/

3. Rename Full Export folder to ExtractExcelFull

4. Extract both files to their folders, overwriting the existing files

5. Download TAXPARCEL19.shp or latest parcel file and overwrite existing
ftp://ftp.mceo.org/Transfer/GIS/Parcel/

> **Careful if downloading a new named file as some of the field names have been changed in the field mapping in the model and will need to be changed again to match the current parcel file in Postgres.**

6. Run the Parcels Update model from the ArcPRO TaxParcelUpdates Project in the GIS\Tax Parcels folder

7. Run Fix Geometries in QGIS on the updated parcel file

8. Import the parcels into Postgres, overwriting existing and converting all fields to lowercase

9. Update the Public Notification AGOL Web Map Layer by overwirting existing service - reads from Postgres

10. Add view permissions to parcels in Postgres to viewer so they will get downloaded during nightly updates

11. Run the vector tile script on the 311 server if need be. 

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
3. Rename Full Export folder to ExtractExcelFull.
4. Extract both files to their folders, overwriting the existing files.
5. Download the last updated TAXPARCEL**.shp or latest parcel file (check the modified date in the ftp) and overwrite existing.
ftp://ftp.mceo.org/Transfer/GIS/Parcel/

6. Run the Parcels Update model from the ArcPRO TaxParcelUpdates Project in the GIS\Tax Parcels folder.

7. Run Fix Geometries in QGIS on the updated parcel file.

8. Import the parcels into Postgres, overwriting existing and converting all fields to lowercase.

9. Update the Public Notification AGOL Web Map Layer from ArcGIS Pro by overwirting existing service - reads from Postgres. - **Add where is this project found??**

10. Add view permissions to parcels in Postgres to viewer so they will be accessible to the web maps.
12. Open the feature server and vector tile server pages to refresh the available layers cache.
13. Refresh the materialized Views that rely on parcels - zoning view, ward lookup view, and geocoder view - starting with the zoning view.

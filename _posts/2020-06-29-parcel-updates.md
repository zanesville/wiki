---
title: Updating the Tax Parcels
tags: arcgispro webmaps
subtitle: Step by Step Process for Updating Parcel Data from the County GIS
---
Download ExtractExcel to Z:\scans\GIS\Tax Parcel\ParcelsMUS\Updates
ftp://ftp.mceo.org/Transfer/GIS/Tax%20Data/

Download Full Export ((also named Extract Excel) to Z:\scans\GIS\Tax Parcel\ParcelsMUS\Updates
ftp://ftp.mceo.org/Transfer/GIS/Tax%20Data/

Rename Full Export folder to ExtractExcelFull

Extract both files to their folders, overwriting the existing files

Download TAXPARCEL19.shp or latest parcel file and overwrite existing
ftp://ftp.mceo.org/Transfer/GIS/Parcel/

Run the Parcels Update model from the ArcPRO TaxParcelUpdates Project in the GIS\Tax Parcels folder

Update the Public Notification AGOL Web Map Layer by overwirting existing service - reads from ??

Import the parcels into Postgres, overwriting existing and converting all fields to lowercase

Add view permissions to parcels in Postgres to viewer so they will get downloaded during nightly updates

---
title: PAVER GIS Exports
tags: postgres
---

The PAVER information is displayed in QGIS and ArcGIS using the dedicated PAVER ``rd_centerlines.shp`` file and Excel spreadsheets exported from PAVER. The spreadsheets are joined to the shapefile using the ``UNIQUEID`` field.

This data is displayed on the web maps using a Postgres View made up of the same shapefile which has been loaded into Postgres as well as the Excel table which has also been loaded into Postgres. To update this layer, you simply need to overwrite the existing ``eng_pci_latest_conditions`` table in Postgres. The centerline file should not need to be updated unless additional streets have been added in PAVER.

## Creating the LatestConditions Export Table

1. Open PAVER
2. Click Reports
3. Choose User Defined Reports
4. Choose LATEST INVENTORY AND CONDITIONS
5. Export the Report by clicking GIS Export, Output Folder z:\GIS\PAVER\Exports
6. Rename the Excel Table from Table.xls to LatestConditions.xls
7. Open the table
8. Rename the only sheet to PCI_Latest
9. Open QGIS
10. Load the table into the map
11. Overwrite the ``eng_pci_latest_conditions`` table in Postgres using DB Manager.

![]({{site.baseurl}}/assets/img/paver_import_pci_to_postgres.jpg)

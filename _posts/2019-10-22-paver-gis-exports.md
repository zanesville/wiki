---
title: PAVER GIS Exports
tags:
- postgres
- paver
category: tasks
---

The PAVER information is displayed in QGIS and ArcGIS using the dedicated PAVER ``rd_centerlines.shp`` file and Excel spreadsheets exported from PAVER. The spreadsheets are joined to the shapefile using the ``UNIQUEID`` field.

This data is displayed on the web maps using a Postgres View made up of the same shapefile which has been loaded into Postgres as well as the Excel table which has also been loaded into Postgres. To update this layer, you simply need to overwrite the existing ``eng_pci_latest_conditions`` table in Postgres. The centerline file should not need to be updated unless additional streets have been added in PAVER.

## Creating the LatestConditions Export Table

### Exporting the Table and Preparing to work in the ArcMap and QGIS Layers
1. Open PAVER
2. Click Reports
3. Choose User Defined Reports
4. Choose LATEST INVENTORY AND CONDITIONS
5. Export the Report by clicking GIS Export, Output Folder z:\GIS\PAVER\Exports
6. Rename the Excel Table from Table.xls to LatestConditions.xls
7. Open the table
8. Rename the only sheet to PCI_Latest

### Creating the Web Map View
1. Open QGIS
2. Load the LatestConditions table into the map.
3. Load the new table into Postgres as ``eng_paver_latest_pci_new`` in using DB Manager.
4. Edit the current ``eng_paver_pci_view``, replacing ``eng_paver_latest_pci`` with ``eng_paver_latest_pci_new``. 
5. Rename the column reference ``predicted_pci_(MO_DD_YYY)`` in two places with the date litsed in the new table.
6. Refresh the view by right clicking the view, refresh view > with data.
7. Delete the old ``eng_paver_latest_pci`` table.
8. Rename the new table back to ``eng_paver_latest_pci`` - pg_admin will automatically update the table name in the view.
9. **IMPORTANT** Add "viewer" user to SELECT priviledges on the view anytime the view is updated.
10. **IMPORTANT** Load both pages below to refresh the data server caches of available layers.
  - [https://311.coz.org/api/v1/feature-server/collections.html](https://311.coz.org/api/v1/feature-server/collections.html)
  - [https://311.coz.org/api/v1/vector-tiles/](https://311.coz.org/api/v1/vector-tiles/)

```SQL
SELECT row_number() OVER () AS id,
  roads.geom,
  roads.lsn AS name,
  pci.pci_category,
  pci.pci,
  pci."predicted_pci_(10_24_2020)" AS pci_predicted,
  pci.predicted_condition_category AS pci_predicted_category,
  pci.last_major_work_date,
  date_part('year'::text, pci.last_major_work_date)::text AS year_last_paved
 FROM eng_paver_centerlines roads,
  eng_paver_latest_pci pci
WHERE roads.uniqueid::text = pci.uniqueid::text;
```

![]({{site.baseurl}}/assets/img/paver_import_pci_to_postgres.jpg)

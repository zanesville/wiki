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
11. Load the new table into Postgres as ``eng_pci_latest_conditions_new`` in Postgres using DB Manager.
11. Copy the SQL from the current ``eng_paver_pci_view``  - below or in Postgres for latest.
12. Delete the old view
13. Delette the old ``eng_pci_latest_conditions``.
14. Rename the new table ``eng_pci_latest_conditions``.
15. Remake the view, adding "viewer" user to SELECT priviledges.
16. Load both pages below to refresh the data server caches of available layers.

```SQL
 SELECT row_number() OVER () AS id,
  roads.geom,
  roads.lsn AS name,
  pci.pci_category,
  pci.pci,
  pci.predicted_condition_category AS pci_predicted_category,
  pci.last_major_work_date,
  date_part('year'::text, pci.last_major_work_date)::text AS year_last_paved
FROM eng_paver_centerlines roads,
  eng_paver_latest_pci pci
WHERE roads.uniqueid::text = pci.uniqueid::text;
```

![]({{site.baseurl}}/assets/img/paver_import_pci_to_postgres.jpg)

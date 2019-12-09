---
title: PostgreSQL & PostGIS
tags: postgis
---

### ArcGIS Desktop Query Layers
> When you access a database table through a Database Connection, query layer, or web service, ArcGIS filters out any unsupported data types. ArcGIS does not display unsupported data types, and you cannot edit unsupported data types through ArcGIS.

(https://pro.arcgis.com/en/pro-app/help/data/geodatabases/manage-postgresql/data-types-postgresql.htm](https://pro.arcgis.com/en/pro-app/help/data/geodatabases/manage-postgresql/data-types-postgresql.htm)
- Using the PostGIS built-in geometry
- Row column ID for ArcGIS and when creating new layers - leave blank - PostGIS or Postgres using "id" and will auto increment this in QGIS if all goes well.

### Tuning Postgres for Spatial
[https://postgis.net/workshops/postgis-intro/tuning.html](https://postgis.net/workshops/postgis-intro/tuning.html)
#### Resources

[https://gist.github.com/justinlewis/4398913](https://gist.github.com/justinlewis/4398913)
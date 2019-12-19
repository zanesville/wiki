---
title: PostgreSQL & PostGIS
tags: postgis
---

#### Split lines with Points
```
CREATE MATERIALIZED VIEW lines_split_mv AS
SELECT a.id, (ST_Dump(ST_split(st_segmentize(a.geom,1),ST_Union(b.geom)))).geom::geometry(LINESTRING) AS geom 
FROM san_lines_post a, san_points b
GROUP BY a.id;
```

Convert a file from one format to another while specifying the destination projection:

```
ogr2ogr -f GeoJSON -t_srs crs:84 lines-new.geojson lines.shp
```

### ArcGIS Desktop Query Layers
> When you access a database table through a Database Connection, query layer, or web service, ArcGIS filters out any unsupported data types. ArcGIS does not display unsupported data types, and you cannot edit unsupported data types through ArcGIS.

(https://pro.arcgis.com/en/pro-app/help/data/geodatabases/manage-postgresql/data-types-postgresql.htm](https://pro.arcgis.com/en/pro-app/help/data/geodatabases/manage-postgresql/data-types-postgresql.htm)
- Using the PostGIS built-in geometry
- Row column ID for ArcGIS and when creating new layers - leave blank - PostGIS or Postgres using "id" and will auto increment this in QGIS if all goes well.

### Tuning Postgres for Spatial
[https://postgis.net/workshops/postgis-intro/tuning.html](https://postgis.net/workshops/postgis-intro/tuning.html)

### Triggers
```sql
Create or replace function calc_length()
returns trigger as
$BODY$
BEGIN
NEW.shape_length := st_length(NEW.geom);
return new;
end;
$BODY$
language plpgsql;

Create trigger calc_length before insert or update on public.utl_stormwater_stm_lines
for each row execute procedure calc_length();
```

### Auto Increment ID

```powershell
ALTER table utl_sanitary_lines drop constraint utl_sanitary_lines_pkey;
alter table utl_sanitary_lines add column uid bigserial primary key;
alter table utl_sanitary_lines drop column id;
alter table utl_sanitary_lines rename column uid to id;
```
#### Resources

[https://gist.github.com/justinlewis/4398913](https://gist.github.com/justinlewis/4398913)
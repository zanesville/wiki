---
title: PostGIS & GDAL Commands Cheat Sheet
tags:
- postgis
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
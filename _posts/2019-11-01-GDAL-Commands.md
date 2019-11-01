---
title: GDAL Commands
---

Convert a file from one format to another while specifying the destination projection:

``
ogr2ogr -f GeoJSON -t_srs crs:84 lines-new.geojson lines.shp
``
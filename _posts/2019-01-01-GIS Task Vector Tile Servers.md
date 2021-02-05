---
title: GIS Vector Tile Servers
category: x-archive
tags: []
---

## Tegola

``./tegola cache seed --bounds -84,38,-81,42 --max-zoom 18 ``

### Benefits
- No tile artifacts like in t_rex or geojson-vt
- Can read from postgis database

### Issues
- Cannot convert from EPSG:3735 to EPSG:3857 (NAD 83 Ohio SP South to Web Mercator)
	- This will mean making a duplicate copy of the data in another projection unless I can query the data out into Web Mercator
- On-the-Fly tile creation is slow compared to t_rex and geojson-vt but it does have a caching option

## geojson-vt NodeJS Server

#### Benefits
- Easy to understand the code
- Simple
- Fast

#### Issues
- Tile rendering issues

## t_rex

#### Issues
- PostgreSQL as a datasource not working
- Tile rendering issues
- Slower than the Node Server

## Geoserver

#### Benefits
- OGC Compliant server with WMS, WFS, WFS-T, raster tiles, vector tiles and and raw data files
- WMS searching for layer search
- Has it's own caching system
- Can use a variety of data sources from PostGIS to shapefiles and more


#### Issues
- Resource heavy
- Complicated setup and configuration

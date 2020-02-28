---
title: Vector Tile Cache
tags: webmaps vector-tiles postgres
---

This is done with a script that runs nightly as a scheduled task on the GIS computer (it could run directly on the server as well). It is located at ``Z:\scans\GIS_Applications\production\geojson-sde-backup-and-vector-tile-cache\index.js``. This script does the following:

>This script does not work with MultiPoint geometry. If it fails try setting ``create single part instead of multi-part`` when uploading to Postgres

1. Spins up a simple Node server to serve out GeoJSON endpoints of all data in the Postgres database with user "viewer" select privledges.
2. Downloads a layer list.
3. Appends the layer list with the timestamp for the last edited date of the layer.
4. Compares the timestamp to a cached layer list - saved as a json file in the same folder as the script.
5. Downloads any changed files as GeoJSON.
6. Compares these files to the GeoJSON files on the 311 server.
7. If new, adds a unique ID to the file, creates a centroid label layer if it is a polygon, and overwrites the 311 GeoJSON cache with these files.
8. Creates a vector tile cache on the 311 server using a modified version of ``geojson2mvt``.
9. An available layers list is updated on the 311 server as is the layerCacheList.json in the script folder.

>Some large files, such as the ``env_contours_10ft_simplified`` are too large to be cached using this script. The vector tiles for any large files such as these were created by first exposting the geojson from QGIS then using the ``vtile`` script directly on this file, increasing the Node memory limit with ``node --max_old_space_size=16384 vtile -f filename -Z 16``
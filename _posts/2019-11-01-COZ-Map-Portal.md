---
title: COZ Map Portal | gis.coz.org
tags: mapbox webmaps
---

>This documentation is not complete!!!!!!

## Creating the Vector Tile Cache

This is done with a script that runs nightly as a scheduled task on the GIS computer (it could run directly on the server as well). It is located at ``Z:\scans\GIS_Applications\production\geojson-sde-backup-and-vector-tile-cache\index.js``. This script does the following:

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


### Software & Building
NodeJS needs to be installed to build and deploy this website.

Major Dependencies
- Parcel
- Hexo
- Gulp

The site is hosted on Netlify and on an internal city server. Use netlify-cli command-line tools to deploy to Netlify, this is taken care of in the ``npm run deploy script``. 

### Maps

### Build Process

### Data Sources

- Mapbox
	- basemap and ortho for one of our apps
- 311.coz.org/data
	- all vector data is derived from the database, updated nightly
	- raster data is created from either QGIS or ArcMap/Pro and copied over manually

### Scripts

```sql
  "tile": "node ./_scripts/tile-data.js",
  "parcel": "parcel build ./source/assets/css/src/main.scss -d ./source/assets/css/dist & parcel build ./source/assets/js/build/coz-scripts-parcel.js --global cozMAP -d ./source/assets/js/dist --out-file coz-scripts.min.js --no-source-maps",
  "parcel-dev": "parcel build ./source/assets/css/src/main.scss -d ./source/assets/css/dist & parcel build ./source/assets/js/build/coz-scripts-parcel.js --no-minify --global cozMAP -d ./source/assets/js/dist --out-file coz-scripts.min.js --detailed-report",
  "docs": "gulp clean-jsdoc & jsdoc source/assets/js/src -c _jsdoc/jsdoc.json -d source/pages/docs",
  "start": "hexo clean & npm run tile & gulp build-mapbox & npm run parcel-dev & hexo serve",
  "serve": "hexo serve",
  "pwa": "sw-precache --config=sw-precache-config.js",
  "test": "hexo clean & npm run tile & gulp build-mapbox & npm run parcel & npm run docs & hexo generate & npm run pwa & http-server -p 4000 -o -c -1",
  "test-deploy": "gulp cdn & hexo deploy",
  "encrypt": "node _encrypt/encrypt.js",
  "deploy": "hexo clean & npm run tile & gulp build-mapbox & npm run parcel & npm run docs & hexo generate & gulp clean-public & gulp cdn & npm run pwa & netlify deploy --prod"
```

## Possible Upgrades
Multi-Select on features
Imagery Historic Slider
Multi-View for popup information, step through
Attribute Table w/Export

## THIS ALL NEEDS UPATED Minimal Setup

### Some way to sync data from my computer to the server

Setting up a git server would be great
 - [https://bonobogitserver.com/install/](https://bonobogitserver.com/install/)
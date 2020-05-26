---
title: City Web Maps
tags: mapbox webmaps
subtitle: A short overview of the city's web apps, web maps and other geospatial applications
---

## Intro

THe main city web maps are hosted on Netlify, powered by Mapbox GL JS, and are built with NodeJS. The main build tools are parcel-bundler, Gulp and the static site generator Hexo. The data is a combination of raster and vector tiles and raw GeoJSON data. The raster tiles are created using QGIS export xyz tiles from a raster *already in WGS84*. The vector tiles and GeoJSON data is created using the process below.

The JavaScript that powers the map UI is a custom-built library. This library is bundled using parcel-bundler. It powers the layer control, popups, select, measure control, etc. The files should be self-explanatory but could be broken out even further into individual components.

>The web maps could all be ported over to ArcGIS Online or the AGOL CMV, but those tools lack some of the functionality in the custom maps.

## ArcGIS Online

A few maps and layers are hosted on AGOL. Several Survey123 apps are hosted here, mainly for field inspections.

## 311.coz.org

This is a Windows IIS Server managed by IT that houses our GIS database as well as a NodeJS Fastify server. The Node server runs a few secured apps that talk directly to the GIS database.

## gis.coz.org

HexoJS (NodeJS) static site hosted on Netlify. Contains the majority of our web maps. More information below.

### Creating the Vector Tile Cache

This is done with a script that runs nightly as a scheduled task on the 311 server. It is located at ``\geospatial-applications\node_applications\production\gis-data-vector-tile-cache``. This script does the following:

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

The site, besides the static vector tiles, raster tiles, and data attachments (PDFs, asset images, etc) is hosted on Netlify, with the other files hosted on the 311 server. Use netlify-cli command-line tools to deploy to Netlify, this is taken care of in the ``npm run deploy script``. 

### Data Sources

- Mapbox
	- basemap and ortho for one of our apps
- 311.coz.org/data
	- all vector data is derived from the database, updated nightly
	- raster data is created from either QGIS or ArcMap/Pro and copied over manually

### Scripts

See the package.json in the project folder.

### Potential Upgrades
Multi-Select on features
Imagery Historic Slider
Attribute Table w/Export
Area select and export of utility features

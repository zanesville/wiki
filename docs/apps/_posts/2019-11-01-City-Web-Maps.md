---
title: City Web Maps
tags: mapbox webmaps
subtitle: A short overview of the city's web apps, web maps and other geospatial applications
---

## Intro

The main city web maps and a handful of apps are (mostly) hosted on Netlify, powered by Mapbox GL JS, and are built with NodeJS. The main build tools are parcel-bundler, Gulp and the static site generator Hexo. The data is a combination of raster and vector tiles and raw GeoJSON data. The raster tiles are created using QGIS export xyz tiles from a raster *already in WGS84*. The vector tiles and GeoJSON data are for the most part served from pg_tileserv and pg_featureserv. Data hosted on AGOL is either pulled directly from the Feature Service or pulled in via GeoJSON or vector tiles that get updated nightly.

>The web maps could all be ported over to ArcGIS Online Maps, ArcGIS Online Maps Beta, or the the Esri open source configurable map viewer (CMV), but those tools lack some of the functionality in the custom maps, in particular decent looking printed maps.

The JavaScript that powers the map controls such as the layer control and network trace control is a custom-built library. This library is bundled using parcel-bundler. The files should be self-explanatory but could be broken out even further into individual components for easier debugging.

## ArcGIS Online

A few maps and layers are hosted on AGOL. Several Survey123 apps are hosted there as well, mainly for field inspections.

## 311.coz.org

This is a Windows IIS Server managed by IT that houses our GIS database, static files for the web maps including raster tiles, vector tiles, and attachments, as well as a NodeJS Fastify server for a handful of custom built CRUD apps. These Node apps talk directly to the GIS database.

## gis.coz.org

HexoJS (NodeJS) static site hosted on Netlify. Contains the majority of our web maps. More information below.

### Creating Vector Tile Cache for ArcGIS Online Feature Services

The AGOL feature and tile cache is built with two NodeJS tools - ``agol-cache`` and ``vtile``.

### Software & Building
NodeJS needs to be installed to build and deploy the main website.

Major Dependencies
- Parcel - installed globally
- Hexo
- Gulp - installed globally

The site, besides the static vector tiles, raster tiles, and data attachments (PDFs, asset images, etc) is hosted on Netlify, with the other files hosted on the 311 server. Use netlify-cli command-line tools to deploy to Netlify, this is taken care of in the ``npm run deploy`` script. 

### Data Sources

- Mapbox
	- Basemap for one of our apps
	
- 311.coz.org/data
	- All vector data is derived from the database
	- Raster data is created from either QGIS and copied over to the 311 server manually

### Scripts

See the package.json in the project folder.

### Potential Upgrades
Multi-Select on features
Imagery Historic Slider
Attribute Table w/Export
Area select and export of utility features

---
title: Web Map Overview
tags: webmaps
subtitle: A short overview of the city's web apps, web maps and other geospatial applications
  managed by GIS
---

## ArcGIS Online

A few maps and layers are hosted on AGOL. Several Survey123 apps are hosted here, mainly for field inspections.

## gis.coz.org

HexoJS (NodeJS) static site hosted on Netlify. Contains the majority of our web maps.

## 311.coz.org

This is a Windows IIS Server managed by IT that houses our GIS database as well as a NodeJS Fastify server. The Node server runs a few secured apps that talk directly to the GIS database.
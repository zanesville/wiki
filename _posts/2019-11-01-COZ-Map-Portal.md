---
title: COZ Map Portal | gis.coz.org
tags: mapbox, test
---

This describes the publication process for the City of Zanesville Map Portal - gis.coz.org.

### Software
NodeJS needs to be installed to build and deploy this website.

Major Dependencies
- Parcel
- Hexo
- Gulp

### Maps

### Build Process

### Host

The site is hosted on Netlify. Use netlify-cli command-line tools to deploy to Netlify, this is taken care of in the ``npm run deploy script``. 

### Data Sources

### Scripts

```cmd
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
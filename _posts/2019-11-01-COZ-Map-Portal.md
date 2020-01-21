---
title: COZ Map Portal | gis.coz.org
tags: mapbox
---

>This documentation is not complete!!!!!!

### Software
NodeJS needs to be installed to build and deploy this website.

Major Dependencies
- Parcel
- Hexo
- Gulp

### Maps

### Build Process

### Host

The site is hosted on Netlify and on an internal city server. Use netlify-cli command-line tools to deploy to Netlify, this is taken care of in the ``npm run deploy script``. 

### Data Sources

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


## THIS ALL NEEDS UPATED Minimal Setup

### Firewall

I would need any firewall setup to allow the node server to talk to the cloud-based managed database

### IIS Reverse Proxy

For the interactive maps I need to proxy certain requests to NodeJS

[https://www.sapho.com/documentation/how-to-configure-microsoft-iis-http-server-as-a-reverse-proxy-for-sapho-server-running-on-apache-tomcat/](https://www.sapho.com/documentation/how-to-configure-microsoft-iis-http-server-as-a-reverse-proxy-for-sapho-server-running-on-apache-tomcat/)


[https://dev.to/petereysermans/hosting-a-node-js-application-on-windows-with-iis-as-reverse-proxy-397b](https://dev.to/petereysermans/hosting-a-node-js-application-on-windows-with-iis-as-reverse-proxy-397b)

### SSL Certificate

Everything needs to run on SSL to make all APIs happy

[https://weblog.west-wind.com/posts/2016/feb/22/using-lets-encrypt-with-iis-on-windows](https://weblog.west-wind.com/posts/2016/feb/22/using-lets-encrypt-with-iis-on-windows)

### Node Modules Globally Installed
- pm2 - globally installed - this manages the node app, restarts it if it crashes, can force it to run on multiple cores, and can restart the app on-demand

### Some way to sync data from my computer to the server

Setting up a git server would be great
 - [https://bonobogitserver.com/install/](https://bonobogitserver.com/install/)
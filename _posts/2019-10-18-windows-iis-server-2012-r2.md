---
title: Windows IIS Server 2012 R2
tags: webmaps
---

## Node Apps Start at Boot
see https://thomasswilliams.github.io/2020/04/07/installing-pm2-windows.html

## Compression

compression types have to be listed before */*

MIME Types for Vector Tiles and GeoJSON

https://github.com/mapbox/vector-tile-spec/issues/48

## Authentication

Authentication Application Pool

## HTTPS & SSL

Be sure to have a valid SSL Certificate installed

Make sure Require SSL **IS NOT CHECKED**

Then use the URL Rewrite add-in as explained in the following post to redirect HTTP to HTTPS

[https://www.sslshopper.com/iis7-redirect-http-to-https.html](https://www.sslshopper.com/iis7-redirect-http-to-https.html)

## Basic Authentication

> HTTP to HTTPS redirect should be set when using Basic Authentication as usernames and passwords are sent in clear text

The users that are allowed to access the folders protected with Authentication are just the local users allowed to access the folder, with the @domain.local after the username.

[https://docs.microsoft.com/en-us/iis/configuration/system.webserver/security/authentication/](https://docs.microsoft.com/en-us/iis/configuration/system.webserver/security/authentication/)

## Node Reverse Proxy/URL Redirect

[https://dev.to/petereysermans/hosting-a-node-js-application-on-windows-with-iis-as-reverse-proxy-397b](https://dev.to/petereysermans/hosting-a-node-js-application-on-windows-with-iis-as-reverse-proxy-397b)


## SSL Certificate

Everything needs to run on SSL to make all APIs happy

[https://weblog.west-wind.com/posts/2016/feb/22/using-lets-encrypt-with-iis-on-windows](https://weblog.west-wind.com/posts/2016/feb/22/using-lets-encrypt-with-iis-on-windows)

## Node Modules Globally Installed
- pm2 - globally installed - this manages the node app, restarts it if it crashes, can force it to run on multiple cores, and can restart the app on-demand
- pm2-windows-service pm2 normally runs as the user, so we need to run it as a service so that is does not shutdown when you logout of the server
	- see [https://github.com/jon-hall/pm2-windows-service#readme](https://github.com/jon-hall/pm2-windows-service#readme)

## CORS

https://enable-cors.org/server_iis7.html



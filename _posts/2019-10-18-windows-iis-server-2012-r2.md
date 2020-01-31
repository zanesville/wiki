---
title: Windows IIS Server 2012 R2
tags: webserver
---

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

## CORS

https://enable-cors.org/server_iis7.html
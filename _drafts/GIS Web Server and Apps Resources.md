---
layout: wiki
title: GIS Web Serer and Apps Resources
---
# Minimal Setup

If I can have access to the server manager I could probably get most of this working, but if IT wants to do this that's fine as well.

### SSH Access to the Server

I could just remote in using remote desktop but it would be great to also be able to use SSH and get access to the terminal on the server

### Firewall

I would need any firewall setup to allow the node server to talk to the cloud-based managed database

### IIS Reverse Proxy

For the interactive maps I need to proxy certain requests to NodeJS

[https://www.sapho.com/documentation/how-to-configure-microsoft-iis-http-server-as-a-reverse-proxy-for-sapho-server-running-on-apache-tomcat/](https://www.sapho.com/documentation/how-to-configure-microsoft-iis-http-server-as-a-reverse-proxy-for-sapho-server-running-on-apache-tomcat/)


[https://dev.to/petereysermans/hosting-a-node-js-application-on-windows-with-iis-as-reverse-proxy-397b](https://dev.to/petereysermans/hosting-a-node-js-application-on-windows-with-iis-as-reverse-proxy-397b)

### SSL Certificate

Everything needs to run on SSL to make all APIs happy

[https://weblog.west-wind.com/posts/2016/feb/22/using-lets-encrypt-with-iis-on-windows](https://weblog.west-wind.com/posts/2016/feb/22/using-lets-encrypt-with-iis-on-windows)

### Password-Protected Folders/Webpages

One of the goals of this project is to have the ability to create password-protected web maps, I assume this is can be done in IIS, hopefully with our current domain usernames and passwords

### Node Modules Globally Installed
- pm2 - globally installed - this manages the node app, restarts it if it crashes, can force it to run on multiple cores, and can restart the app on-demand

### Some way to sync data from my computer to the server

Setting up a git server would be great
 - [https://bonobogitserver.com/install/](https://bonobogitserver.com/install/)


Other options
 - Web Deployment Tool?
 - rsync robocopy

<hr>

## Things I might need but not sure, or could need in the future...

### Caching

I need to cache the NodeJS requests - or do I??

Not sure if the built-in server cache will cache proxied requests or if we need iisnode
[https://github.com/Azure/iisnode](https://github.com/Azure/iisnode)

### Run processes on a schedule

I need to restart pm2 (see below) every night, I assume this could be done with scheduled tasks, not sure what that looks like in Windows Server - in Linux I would just use a cron job
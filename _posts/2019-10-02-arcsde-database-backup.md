---
title: ArcSDE Database Setup & Backup
subtitle: GIS data is now stored in Postgres and editing in QGIS. This could still
  be used in the future.
category: archive
---

## Postgres Database

The main PG database is backed up nightly offsite just all other city servers. A simple full backup can also be created using pgadmin.

## In ArcCatalog Enable Editor Tracking

1. Load data into the database
2. Set permissions to the database
2. Batch register data as versioned
	3. [https://pro.arcgis.com/en/pro-app/help/data/geodatabases/overview/a-quick-tour-of-registering-and-unregistering-data-as-versioned.htm](https://pro.arcgis.com/en/pro-app/help/data/geodatabases/overview/a-quick-tour-of-registering-and-unregistering-data-as-versioned.htm)
3. Batch enable editor tracking

Numbers 3 and 4 can be done with with the  ``enableVersioningAndEditorTracking`` model in the **ArcSDESetupAndBackupTools toolbox**.

> Z:\scans\GIS_Tools\ToolboxesArcGISDesktop

![](/assets/img/sdebackup.jpg)

## FileGeodatabase Backup - NO LONGER USED
This model copies all the features inside an SDE database to an existing File Geodatabase with the day of the week in the name. It also writes all the data out to GeoJSON, and copies changed GeoJSON to the web server along with the new vector tiles. A similar model is needed to backup the tables inside the database as well. This model is run from inside a Node script using Python, on a schedule in Windows Task Scheduler.

**It is important to set the alias on the Model and the Toolbox for use in the python script!**

```javascript
let {PythonShell} = require('python-shell');
let createVectorTileCache = require("./src/tile-data")

console.log("attempting to call the python script to backup the sqlexpress database to a FGDB")

let shell = new PythonShell('C:/Users/malcolm.meyer/Desktop/test.py', { 
  mode: 'text',
  pythonPath: 'C:/Python27/ArcGIS10.7/python.exe'
});
shell.on('message', function (message) {
  //CREATE LOG FROM THE MESSAGES
  console.log(message)
});

shell.end(function (err) {
  if (err){
      throw err;
  };
  console.log('finished backing up data using the python script \n attempting to create vector tile cache');
  createVectorTileCache();
});
```

![](/assets/img/fgdb-backup.jpg)

## References

Database Administration Workflow

[https://pro.arcgis.com/en/pro-app/help/data/geodatabases/overview/recommended-version-administration-workflow.htm](https://pro.arcgis.com/en/pro-app/help/data/geodatabases/overview/recommended-version-administration-workflow.htm)

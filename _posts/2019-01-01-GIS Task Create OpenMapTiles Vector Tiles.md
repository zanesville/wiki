---
title: OpenMapTile Vector Tiles
tags: webmaps vector-tiles
category: archive
---

This post outlines how you could use the OpenMapTiles project to create a custom OpenStreetMap extract of vector tiles for use in our maps. We use so little credits on Mapbox that this has not been utilized.

### In Ubunutu 18.04

```powershell
git clone https://github.com/openmaptiles/openmaptiles.git
cd openmaptiles
./quickstart.sh
```

- Delete the albania osm file in the data folder and replace with the latest Ohio data from [http://download.geofabrik.de](http://download.geofabrik.de)
- Move this file into the data folder
- Create a docker-compose-config.yml file in the same directory as follows:

```powershell
version: "2"
services:
  generate-vectortiles:
    environment:
      BBOX: "-84.8202, 38.4031, -80.5187, 41.9775"
      OSM_MAX_TIMESTAMP: "2019-04-04"
      OSM_AREA_NAME: "ohio"
      MIN_ZOOM: 0
      MAX_ZOOM: 14
```

``./quickstart.sh the name you gave in the OSM_AREA_NAME so ./quickstart.sh ohio``

Copy the tiles.mbtiles to Windows

Unpack the mbtiles to a folder of vector tiles with [https://github.com/phiphou/mbtiles2ungzpbf](https://github.com/phiphou/mbtiles2ungzpbf)

---
### Tried on windows and failed

``docker-compose up -d postgres``

```powershell
docker-compose run import-water
docker-compose run import-natural-earth
docker-compose run import-lakelines
docker-compose run import-osmborder
```

#### Install OpenMapTiles-Tools
```powershell
docker run -v data/ohio-latest.osm.pbf openmaptiles/openmaptiles-tools
```


```powershell
docker-compose run --rm openmaptiles-tools generate-tm2source openmaptiles.yaml --host="postgres" --port=5432 --database="openmaptiles" --user="openmaptiles" --password="openmaptiles" > build/openmaptiles.tm2source/data.yml
```

```powershell
docker-compose run --rm openmaptiles-tools generate-imposm3 openmaptiles.yaml > build/mapping.yaml
```

**These two files mapping.yaml and data.yaml need converted to UTF8 which can be done in Notepad or Notepad++**

```powershell
docker-compose run --rm openmaptiles-tools generate-sql openmaptiles.yaml > build/tileset.sql
```




#### Import the ohio-latest.osm.pbf

```powershell
docker-compose run import-osm`
```

This should show that it is importing from the ohio file, and not the default albania file.

#### Generate the mbtiles vector tiles

```powershell
docker-compose run generate-vectortiles
```
- **ERROR - z(numeric) does not exist - Just run on an Ubuntu droplet**

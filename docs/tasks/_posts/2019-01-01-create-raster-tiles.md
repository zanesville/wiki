---
title: Create Raster Tiles
tags: rasters
---

## Creating Raster Tiles

## Preferred Method

### Using QGIS & Generate XYZ Tiles

1. Georeference and Export the raster using the Web Mercator 3857 Projection (256 for No DATA) in ArcMap or ArcPro using ITRF00 transformation. QGIS is missing the ITRF00 transformation. It is hardcoded into PostGIS but not in QGIS, which is why we use Arc* here.
2. Load the raster into QGIS
3. Change the zoomed in sampling to Average and any other settings under properties
4. Export to raster tiles using the Generate XYZ tiles tool
5. See the screenshot with settings - all defaut except make sure to use the extent of the raster layer
6. Use maxzoom 16 for old paper maps, 20 for satellite imagery
    7. Each additional zoom inceases the total tiles by ``previous zoom ^ 2``
7. Copy the tiles folder to the current raster server static data directory, renaiming to match the schema of the other folders. Currently this directory is:
    8. ``\\311 server IP address\wwwroot\data\rasters\``

![](/assets/img/generate_xyz_tiles.jpg)

---

## Other Methods

### Using gdal2tilesp.py

Reproject the original image to EPSG:3857 using gdalwarp. Use all the same options as before expcept use a higher compression. **This needs to be checked to make sure that the geographic transformation is happening correctly.** *This might not make that much difference if using JPG tiles as final output.*

 ```javascript
 gdalwarp -of GeoTIFF -co COMPRESS=JPEG -co JPEG_QUALITY=60 ...
 ```

Use the customized gdal2tilesp.py python script to cut the tif into a folder of raster tiles for use in web mapping. There are similar scripts that will output to mbtiles or geopackage. This script was originally written by the developer of [MapTiler](https://www.maptiler.com/). The gdal2tiles that is built in to gdal only outputs images in png format and **does not use parallel processing**. The default for this updated script is to use all the availabel cpu cores, so be careful when running it. Set the ``--processes`` to something you can live with, such as half the available cores. A maximum zoom limit of 20 seems to be a good limit, which is about street level.

```python
python gdal2tilesp.py -f JPEG -e -z 0-20 --processes=4 input.tif outputFolder
```

*JPEG results in smaller files but looses the transparency.*

**30GB .sid file => 6.56GB tif (JPG_QUALITY 50% using gdalwarp) => 15 GB raster tiles (PNG)**
**30GB .sid file => 6.56GB tif (JPG_QUALITY 50% using gdalwarp) => 4 GB raster tiles (JPG)**

#### References
 - [https://alastaira.wordpress.com/2011/07/11/maptiler-gdal2tiles-and-raster-resampling/](https://alastaira.wordpress.com/2011/07/11/maptiler-gdal2tiles-and-raster-resampling/)
 - [https://github.com/pramsey/gdal2tilesp](https://github.com/pramsey/gdal2tilesp)
 - [https://pvanb.wordpress.com/2017/03/06/raster2mbtiles/](https://pvanb.wordpress.com/2017/03/06/raster2mbtiles/)

### Using gdal_gdaladdo


Use gdal_translate to generate the mbtiles file

```
gdal_translate -of mbtiles mymap.tif mymap.mbtiles
```

Use gdal_gdaladdo to generate the rest of the tiles (overview tiles)

```
gdaladdo -r nearest mymap.mbtiles 2 4 8 16
```
	- this results in tiles from z levels??

Unpack the mbiles to folders using [mbutil](https://github.com/mapbox/mbutil) which is a python script, run it using cmd.exe in its own folder like this:
	``python mb-util``

## Clipping and Resizing using GDAL or QGIS

**Two images will be created, one in the original projection Ohio SP South - EPSG:3735 with a JPEG quality of 75. This one is for desktop GIS use. The second image will be in Web Mercator EPSG:3857, with a JPEG quality between 50 and 60, which will be used to create the image tile cache for web applications.** *This may not matter if using JPEG for the final output raster tiles.*

Saving using LZW or DEFLATE resulted in a very large tif. Using ``-co compression=JPEG -co JPEG_QUALITY=75`` results in great image quality in a much smaller file. Settings used:

```
-multi 			// Uses two processes to manipulate the images
-wo			// DOES NOT SEEM TO BE WORKING Total number of CPUs for calculations
-cutline		// Shapefile to clip the polygon referenced with the full file path in quotes
-crop_to_cutline	// Clip the raster using the cutline polygon 
-co			// Compression used and options
-dstalpha		// Use an Alpha layer to store transparency - the original channel is used if none is provided elsewhere

```

```
gdalwarp -multi -wo NUM_THREADS=8 -of GTiff -cutline E:\ortho18\clip-coz.shp -crop_to_cutline -dstalpha -co COMPRESS=JPEG -co JPEG_QUALITY=75 "E:/ortho18/Area Wide Mosaics/OHMUSK18-SID-3INCH/OHMUSK18-SID-3INCH.sid" E:/ortho18/qgis-testing/OHMUSK18_3IN_GDALWARP_CLIP_JPG75_ALPHA.tif
```

```
gdalwarp -wo NUM_THREADS=ALL_CPUS -multi -of GTiff -cutline E:\ortho18\clip-coz.shp -crop_to_cutline -dstalpha -co COMPRESS=JPEG -co JPEG_QUALITY=75 -co TILED=yes -co BIGTIFF=YES "E:/ortho18/Area Wide Mosaics/OHMUSK18-SID-3INCH/OHMUSK18-SID-3INCH.sid" E:/ortho18/qgis-testing/OHMUSK18_3IN_GDALWARP_CLIP_JPG75_ALPHA_BIGTIFF.tif
```

If an error comes up about the size being too large try adding these settings - 

``-co TILED=yes -co BLOCKXSIZE=256 -co BLOCKYSIZE=256`` - **started task at 11:30 on march 26th**

#### References

[https://www.gdal.org/frmt_gtiff.html](https://www.gdal.org/frmt_gtiff.html)

[https://www.gdal.org/gdalwarp.html](https://www.gdal.org/gdalwarp.html)

[https://github.com/dwtkns/gdal-cheat-sheet](https://github.com/dwtkns/gdal-cheat-sheet)

[https://www.azavea.com/blog/2018/10/11/creating-leaflet-tiles-from-open-data/](https://www.azavea.com/blog/2018/10/11/creating-leaflet-tiles-from-open-data/)

[http://www.thesjg.com/2013/03/generating-aerial-tiles-from-naip-imagery/](http://www.thesjg.com/2013/03/generating-aerial-tiles-from-naip-imagery/)

[https://gis.stackexchange.com/questions/104453/why-result-of-merge-of-multiple-raster-is-so-big](https://gis.stackexchange.com/questions/104453/why-result-of-merge-of-multiple-raster-is-so-big)

[http://docs.geonode.org/en/master/tutorials/advanced/adv_data_mgmt/adv_raster/processing.html](http://docs.geonode.org/en/master/tutorials/advanced/adv_data_mgmt/adv_raster/processing.html)
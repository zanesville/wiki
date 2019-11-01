---
title: Ortho Image Manipulation
---

## Clipping and Resizing using GDAL or QGIS

** Two images will be created, one in the original projection Ohio SP South - EPSG:3735 with a JPEG quality of 75. This one is for desktop GIS use. The second image will be in Web Mercator EPSG:3857, with a JPEG quality between 50 and 60, which will be used to create the image tile cache for web applications.**

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

### References

[https://www.gdal.org/frmt_gtiff.html](https://www.gdal.org/frmt_gtiff.html)

[https://www.gdal.org/gdalwarp.html](https://www.gdal.org/gdalwarp.html)

[https://github.com/dwtkns/gdal-cheat-sheet](https://github.com/dwtkns/gdal-cheat-sheet)

---
## Creating Image Tiles for the Web Viewers

### Use gdal2tiles or gdaladdo

#### Using gdal2tiles

1. Reproject the original image to EPSG:3857 using gdalwarp. Use all the same options as before expcept use a higher compression.
	- ``gdalwarp -of GeoTIFF -co COMPRESS=JPEG -co JPEG_QUALITY=60 ...``
2. Use the customized gdal2tilesp.py python script to cut the tif into a folder of tiles for use in web mapping. There are similar scripts that will output to mbtiles or geopackage, but for now we are using simply a folder of image files. This script was originally written by the developer of MapTiler. The gdal2tiles that is built in to gdal only outputs images in png format and does not use parallel processing. The default for this updated script is to use all the availabel cpu cores, so be careful when running it or set --processes to something like half the available cores to not lock up your machine.
 	- ``python gdal2tilesp.py -f JPEG -e -z 0-19 input.tif outputFolder``

References
 - [https://alastaira.wordpress.com/2011/07/11/maptiler-gdal2tiles-and-raster-resampling/](https://alastaira.wordpress.com/2011/07/11/maptiler-gdal2tiles-and-raster-resampling/)
 - https://github.com/pramsey/gdal2tilesp
 - [https://pvanb.wordpress.com/2017/03/06/raster2mbtiles/](https://pvanb.wordpress.com/2017/03/06/raster2mbtiles/)

1.	Reproject the original image to EPSG:3857 using gdalwarp. Use all the same options as before expcept use a higher compression.
	- ``gdalwarp -of GeoTIFF -co COMPRESS=JPEG -co JPEG_QUALITY=60 ...``
2.	Use gdal_translate to generate the mbtiles file
	- ``gdal_translate -of mbtiles mymap.tif mymap.mbtiles``
3.	Use gdal_gdaladdo to generate the rest of the tiles (overview tiles)
	- ``gdaladdo -r nearest mymap.mbtiles 2 4 8 16``
	- this results in tiles from z levels??
	- unpack the mbiles to folders using [mbutil](https://github.com/mapbox/mbutil) which is a python script, run it using cmd.exe in its own folder like this:
	``python mb-util``
4.	Alternative - Upload the image to Mapbox (50GB free tile hosting) and cross fingers that it processes without timing out!

### References

[https://www.azavea.com/blog/2018/10/11/creating-leaflet-tiles-from-open-data/](https://www.azavea.com/blog/2018/10/11/creating-leaflet-tiles-from-open-data/)

[http://www.thesjg.com/2013/03/generating-aerial-tiles-from-naip-imagery/](http://www.thesjg.com/2013/03/generating-aerial-tiles-from-naip-imagery/)

[https://gis.stackexchange.com/questions/104453/why-result-of-merge-of-multiple-raster-is-so-big](https://gis.stackexchange.com/questions/104453/why-result-of-merge-of-multiple-raster-is-so-big)

[http://docs.geonode.org/en/master/tutorials/advanced/adv_data_mgmt/adv_raster/processing.html](http://docs.geonode.org/en/master/tutorials/advanced/adv_data_mgmt/adv_raster/processing.html)

<div class="divider" style="border-color:lightgray;"></div>

### [Mapbox GeoTIFF Upload Recommendataions](https://docs.mapbox.com/help/troubleshooting/uploads/)

### TIFF uploads
There are a couple requirements for TIFFs:

Only 8-bit GeoTIFFs are supported. Run gdalinfo to find your GeoTIFF's resolution.
Mapbox only accepts TIFFs with georeferencing information (GeoTIFFs). Make sure your TIFF is georeferenced before trying to upload.
If you are attempting to upload large TIFFs (multi GBs), here are some ways you can optimize your TIFF before uploading:

- Reproject to Web Mercator EPSG:3857.
- Set blocksize to 256x256.
- If compression is needed, use LZW.
- Remove Alpha band, if applicable.
- 10GB Limit
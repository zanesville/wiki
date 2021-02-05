---
title: Creating Contours in QGIS from a DEM
tags: QGIS
category: tasks

---

The ``gdal`` contour tool only works on an entire DEM, so if this is not desired, the raster needs to be clipped first.

1. Clip the Raster using the clip tool from the processing toolbox.
2. From the QGIS toolbar, choose Raster->Extraction->Contour, using the appropriate interval for the desired contorurs. This will be in the units of the projection of the raster. For DEMs downloaded from OGRIP, these units will be in feet.
3. The resulting contours will be very detailed. To get a traditional smoothed topographic line the contours first need to be simplified, then this simplified output will be smoothed.
  3. Use the `Simplify` tool, using a tolerance of 2. Try a higher simplification tolerance if needed.
  3. Use the ``Smooth`` tool using 5 iterations and defaults for all other settings. Use higher iterations if needed.

*I attempted the recommendation below but it error out.*

### Resources

[https://gis.stackexchange.com/questions/276897/producing-smooth-and-consistent-contour-lines-from-srtm](https://gis.stackexchange.com/questions/276897/producing-smooth-and-consistent-contour-lines-from-srtm)

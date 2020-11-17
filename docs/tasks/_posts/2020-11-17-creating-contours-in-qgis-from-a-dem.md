---
title: Creating Contours in QGIS from a DEM
tags: QGIS
---

The ``gdal`` contour tool only works on an entire DEM, so if this is not desired, the raster needs to be clipped first.

1. Clip the Raster using the clip tool from the processing toolbox.
2. From the QGIS toolbar, choose Raster->Extraction->Contour, using the appropriate interval for the desired contorurs. This will be in the units of the projection of the raster. For DEMs downloaded from OGRIP, these units will be in feet.

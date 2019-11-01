---
title: ArcGIS Online Collector Setup
tags: agol
---
<span class="bg-error">Esri's Collector does not allow for snapping lines to points.</span>

## All about Datum Transformations

All data collected using ArcMap and published to the web via GeoJSON should be transformed using the IRTF00 transformation (the default in Pro, needs to be set in ArcMap). 

For data collected with a GPS unit connected to the ODOT RTK or some other correction network, **according to [this post](https://community.esri.com/thread/225752-issues-with-wgs1984itrf00tonad1983-datum-transformation) by an Esri staffer:**
> ...if the data went through any RTK or post-processing, it was aligned with the control points that were used--usually either an ITRF system or some realization of NAD83. At that point, the data is in the same coordinate system as the control point network, but software sometimes doesn't say that.

This seems to indicate that the data can be used as-is, at least according to this author.

## References

[Custom IRTF00 PostGIS Transformation](https://gis.stackexchange.com/questions/112198/proj4-postgis-transformations-between-wgs84-and-nad83-transformations-in-alask?rq=1)

[Shift Toward More Accurate Data](http://proceedings.esri.com/library/userconf/seuc18/papers/seuc-31.pdf)

[Location Profiles in Collector](https://www.seilergeo.com/2017/07/21/location-profile-setup-for-real-time-corrections-with-esri-collector/)

[http://www.dot.state.oh.us/Divisions/Engineering/CaddMapping/Survey/Pages/VRSRTK-.aspx](http://www.dot.state.oh.us/Divisions/Engineering/CaddMapping/Survey/Pages/VRSRTK-.aspx)
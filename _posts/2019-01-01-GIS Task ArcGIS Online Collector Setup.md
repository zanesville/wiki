---
title: ArcGIS Online Collector Setup
tags: agol webmaps gps
active: true
---

## ODOT VRS RTK Setup

- Apply for a free password from ODOT - [more information](http://www.dot.state.oh.us/Divisions/Engineering/CaddMapping/Aerial/Pages/VRSRTK.aspx)
- Setup the Collector profile - each user needs their own profile
- Use 3735 NAD 83 SP South, Transformation ITRF00 or ITRF08
- Use Mountpoint ODOT_VRS_RTCM23??????

<span class="bg-error">Esri's Collector (not classic) on iOS now allows for snapping lines to points. This is not yet in Collector for Android.</span>

## Datum Transformations

Collector maps use the Web Mercator projection. When you upload data to from ArcGIS Pro to AGOL, **I belive it is transformed with the default transformation - ITRF00 for NAD83**. Then when this is downloaded again, it is transformed back - not 100% sure of what transformation it uses when converting back but probably the default in ArcGIS Pro, which is the ITRF00.

For data collected with a GPS unit connected to the ODOT RTK or some other correction network, **according to [this post](https://community.esri.com/thread/225752-issues-with-wgs1984itrf00tonad1983-datum-transformation) by an Esri staffer:**
> "...if the data went through any RTK or post-processing, it was aligned with the control points that were used--usually either an ITRF system or some realization of NAD83. At that point, the data is in the same coordinate system as the control point network, but software sometimes doesn't say that." - This is why you must set the transformation in Collector. It transforms the RTK point to Web Mercator. Again, this is transformed back to the original projection of the data when exported from AGOL.

Data collected outside of Collector, but with an RTK connection, needs transformed to WGS84 with the ITRF00 projection to be viewed properly on a web map that is in WebMercator (such as Mapbox maps). It would then need to be transformed back to the original projection if downloaded say as WGS84 GeoJSON from inside the web map.

## Collector and Field Inspections

1. Create or use an existing feature in a FileGeodatabase
2. Create a table for inspections in the **same** database
3. Add GlobalIDs to the feature(s)
4. Add a field for the parent key in the inspection table and use the GUID for the type
5. Optionally enable GlobalIDs for the inspection table
6. Enable editor tracking - user AGOL defaults for fields in Survey123 or if created in AGOL (optional can do this in AGOL) - use batch mode - right click on tool
	7. Creator
	8. CreationDate
	9. Editor
	10. EditDate
11.  Add a simple relationship class with no messaging
12.  Share to AGOL
13.  Any changes to the underlying service in ArcGIS Pro CANNOT CHANGE THE ORDER OF LAYERS OR ADD NEW LAYERS TO THE TOP OF THE LAYER LIST!!
14.  The feature services are numbered from the layer list - so if you add a new layer to anywhere but the very bottom of the layer list and attempt to overwrite the existing AGOL service definition, it might fail or if it succeeds, it will create new numbers for the features services, hence breaking any existing maps.

## References

[Geographic Transformation Table with Accuracies](https://desktop.arcgis.com/en/arcmap/latest/map/projections/pdf/geographic_transformations.pdf)

[Custom IRTF00 PostGIS Transformation](https://gis.stackexchange.com/questions/112198/proj4-postgis-transformations-between-wgs84-and-nad83-transformations-in-alask?rq=1)

[Shift Toward More Accurate Data](http://proceedings.esri.com/library/userconf/seuc18/papers/seuc-31.pdf)

[Location Profiles in Collector](https://www.seilergeo.com/2017/07/21/location-profile-setup-for-real-time-corrections-with-esri-collector/)

[http://www.dot.state.oh.us/Divisions/Engineering/CaddMapping/Survey/Pages/VRSRTK-.aspx](http://www.dot.state.oh.us/Divisions/Engineering/CaddMapping/Survey/Pages/VRSRTK-.aspx)

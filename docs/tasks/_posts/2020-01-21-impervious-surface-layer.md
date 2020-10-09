---
title: Impervious Surface Layer
tags: postgis
subtitle: Explanation of the history and creation of the Impervious Surface layer
  and the public layer view
---

The Impervious Surface layer was created in 2019. It was originally put together using aerial photos and field visits by students at Ohio University's Voinivich Center. The city finalized the dataset. 

The master layer is broken out by parcel, billing account number, and partially by surface type. To get the final ERU, the features are dissolved on the account number. This was previously done with a complicated ArcMap model, but is now done directly in Postgres. The view will not update automatically, but can be updated by right clicking on the view in the QGIS 'Browser' and clicking refresh view.

The layer consists of a polygon layer and a related table. The table keeps track of ERU credits and apartment credits. 

In the following SQL definition, the "3" value is the current ERU fee. Billing keeps track of this fee themselves based on the ERU they have on file, so the ``imp_fee`` field is only used for GIS purposes, in case for example we want to quickly see the impervious fee for a property.

## Creating the Master Dissolved Impervious Layer

```sql
drop materialized view if exists utl_stormwater_impervious_view;

create materialized view utl_stormwater_impervious_view as
select
	row_number() over () as id,
	ST_Union(geom) as geom,
	imp_master,
	imp_accnt,
	sum(st_area(geom)) as imp_total_area,
	round(sum(st_area(geom) / 2300)) as imp_eru,
	credits as imp_credits,
	units as imp_units,
	greatest(((round(sum(st_area(geom) / 2300)) - units - credits) * 3), 3 * (1 - units - credits), 0) as imp_fee
from utl_stormwater_impervious, utl_stormwater_impervious_billing
where imp_type != 'EXEMPT' and utl_stormwater_impervious.imp_accnt = "UTL_ACCNT"
group by imp_accnt, imp_master, units, credits;

create unique index on utl_stormwater_impervious_view (id);
```

### Resources
[https://gis.stackexchange.com/questions/229367/st-union-fails-with-topologyexception-despite-valid-polygons-and-using-st-snapto](https://gis.stackexchange.com/questions/229367/st-union-fails-with-topologyexception-despite-valid-polygons-and-using-st-snapto)
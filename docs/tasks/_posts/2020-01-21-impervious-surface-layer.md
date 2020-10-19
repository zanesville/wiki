---
title: Impervious Surface Layer
tags: postgis
subtitle: Explanation of the history and creation of the Impervious Surface layer
  and the public layer view
---

The Impervious Surface layer was created in 2019. It was originally put together using aerial photos and field visits by students at Ohio University's Voinivich Center. The city finalized the dataset. The layer is only used to update the billing database. It is not tied directly to billing. The billing database calculates the ERU fee based on credits and aparment units under the same top level account, if any.

The polygon layer is broken out by parcel, billing account number, and partially by surface type. To get the final ERU, the features are dissolved on the account number. This was previously done with a complicated ArcMap model, but is now done directly in Postgres via a View layer, which is a layer stored in memory so it is always up to date.

The layer consists of a polygon layer and a related table. The table keeps track of ERU credits, apartment credits and billing info. 

In the following SQL definition, the "3.5" value is the current ERU fee. Billing keeps track of this fee themselves based on the ERU they have on file, so the ``imp_fee`` field is only used for GIS purposes, in case for example we want to quickly see the impervious fee for a property.

## Creating the Master Dissolved Impervious Layer

```sql
drop view if exists utl_stormwater_impervious_view;

create view utl_stormwater_impervious_view as
 SELECT row_number() OVER () AS id,
    st_multi(st_union(utl_stormwater_impervious.geom))::geometry(MultiPolygon,3735) AS geom,
    utl_stormwater_impervious.imp_master,
    utl_stormwater_impervious.imp_accnt,
    sum(st_area(utl_stormwater_impervious.geom)) AS imp_total_area,
    round(sum(st_area(utl_stormwater_impervious.geom) / 2300::double precision)) AS imp_eru,
    utl_stormwater_impervious_billing.credits AS imp_credits,
    utl_stormwater_impervious_billing.units AS imp_units,
    GREATEST((round(sum(st_area(utl_stormwater_impervious.geom) / 2300::double precision)) - utl_stormwater_impervious_billing.units::double precision - utl_stormwater_impervious_billing.credits::double precision) * 3.5::double precision, (3.5 * (1 - utl_stormwater_impervious_billing.units - utl_stormwater_impervious_billing.credits)::numeric)::double precision, 0::double precision) AS imp_fee
   FROM utl_stormwater_impervious,
    utl_stormwater_impervious_billing
  WHERE utl_stormwater_impervious.imp_type::text <> 'EXEMPT'::text AND utl_stormwater_impervious.imp_accnt::text = utl_stormwater_impervious_billing."UTL_ACCNT"::text
  GROUP BY utl_stormwater_impervious.imp_accnt, utl_stormwater_impervious.imp_master, utl_stormwater_impervious_billing.units, utl_stormwater_impervious_billing.credits;
```

### Resources
[https://gis.stackexchange.com/questions/229367/st-union-fails-with-topologyexception-despite-valid-polygons-and-using-st-snapto](https://gis.stackexchange.com/questions/229367/st-union-fails-with-topologyexception-despite-valid-polygons-and-using-st-snapto)

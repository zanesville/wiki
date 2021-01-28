---
title: Updating Zoning Codes
tags: postgres
---

> Follow these steps to update the Zoning codes for city parcels. It may be possible that you need to first update the parcel layer if there is a parcel split or join.

## Updating the Zoning Code and other Attributes

1. Open the zoning update app
2. Click on the zoning parcel
3. Click edit
4. Update the information
5. Click save

## Updating the Zoning Materialized View
*This is the layer that powers the web maps and the ward lookup tool.*

1. Open pg_admin
2. Right click on ``dev_zoning_mview``
3. Click "Refresh View with Data"
4. No need to update the index on this view as it is updated automatically.

## Updating the COZ Ward Info Materialized View
*This is the layer that powers ward info lookup tool on the website.*

1. Open pg_admin
2. Right click ``eng_coz_ward_lookup_mview``
3. Click "Refresh View with Data"

> Before I created an index on the zoning materialized view, the ward query took forever. With the index it only takes 3 seconds or so.

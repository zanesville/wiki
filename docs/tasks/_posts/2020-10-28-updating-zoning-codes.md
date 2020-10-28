---
title: Updating Zoning Codes
tags: postgres
---
## Updating the Zoning Code and other Attributes

1. Open the zoning update app
2. Click on the zoning parcel
3. Click edit
4. Update the information
5. Click save

## Updating the Zoning View Layer for Web Maps

1. Open pg_admin
2. Right click on ``dev_zoning_mview``
3. Click "Refresh View with Data"
4. No need to update the index on this view as it is updated automatically.

## Updating the COZ Ward Info Layer for the COZ Info Tool

1. Open pg_admin
2. Right click ``eng_coz_ward_lookup_mview``
3. Click "Refresh View with Data"

> Before I created an index on the zoning materialized view, the ward query took forever. With the index it only takes 3 seconds or so.

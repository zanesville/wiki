---
title: Updating Zoning Codes
tags: postgres zoning
category: tasks
---

> Follow these steps to update the Zoning codes for city parcels. It may be possible that you need to first update the parcel layer if there is a parcel split or join.

## Update the Zoning Code and other Attributes

1. Open the zoning update app - see the 311 server for ideas on the location
2. Login with your windows credentials - Lisa can give you R/W access to this folder
3. Turn on the parcels
4. Click on the zoning parcel
5. Click edit
6. Update the information
7. Click save

## Update the Zoning Materialized View
*This is the layer that powers the web maps and the ward lookup tool.*

1. Open pg_admin
2. Right click on ``dev_zoning_mview``
3. Click "Refresh View with Data"
4. No need to update the index on this view as it is updated automatically.

## Update the COZ Ward Info Materialized View
*This is the layer that powers ward info lookup tool on the website.*

1. Open pg_admin
2. Right click ``eng_coz_ward_lookup_mview``
3. Click "Refresh View with Data"
4. If this takes longer than 5 seconds or so there may be an issue with one of the indexes such as the parcel layer index. It usually takes just under 4 seconds.

> Before I created an index on the zoning materialized view, the ward query took forever. With the index it only takes 3 seconds or so.

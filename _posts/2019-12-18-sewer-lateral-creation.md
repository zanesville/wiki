---
title: Sewer Lateral Creation
tags: sewers python models
subtitle: Using ArcMap's Model Builder to Create Laterals from Inspection Camera Observations
---

>The following model will create laterals of 20 feet at a 90 degree angle to the sanitary line. The inspection direction is not exported with the observations, so the model has to be run separately for the downstream inspections and the upstream inspections. The obervation points need to already be snapped to the sanitary lines.

### Steps

1. From WinCan - connect to the WinCan Map.
2. Highlight the Observation points, then  Layer > then Export, making sure to set the projection to 3735.
3. In ArcMap - load a shapefile of the sanitary line layer ``utl_sanitary_lines`` (export from original if need be)
4. Add the observation points to ArcMap
5. Add the following definition query to the observation points, adjusting the OBS_OPCODE to capture all taps if need be
```sql
"OBS_OPCODE" IN ('TBA', 'TFA', 'TFB', 'TFC')
```
6. Select all the observation points that intersect the sanitary lines layer.
7. Switch the selection, being sure to snap everything that did not snap to the sanitary lines layer. (Use integrate at your own risk.)
8. Select the observations collected while running downstream
9. Run the Lateral_Creation model in the WinCan_Models.tbx toolbox against these observations, leaving the ``Downstream`` checkbox checked
10. Do the same for the upstream observations, unchecking the ``Downstream`` box
11. Join the created laterals to the observation points on the OBS_PK field
12. Copy over the DISTANCE field from the observations to the DISTANCE field in the laterals
13. Copy over the OBS_OBSERV field to the NOTES field in the laterals
14. Manually fill in the DIAMETER field
15. Manually fill in the Class field with either "Lateral" or "Storm"
16. Manually fill in the VIDEO_TIME field and VIDEO_LINK field
17. Manually fill in the PROJECT field (using the name of the WinCan project)
18. Manually add the VIDEO_LINK to the AS YET UNKNOWN FIELD MAYBE INSPECT_LINK to the sanitary line layer (in QGIS)
19. Append the laterals to the master utl_sanitary_laterals feature in the Enterprise database (in QGIS)
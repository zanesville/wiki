---
title: Sewer Lateral Creation
tags: sewers python models automation
---

>The following steps will snap the observation points to the existing sanitary lines with a 3 foot tolerance. It may be necessary to adjust the original lines to account for any errors in the existing dataset. Then the model will then create laterals of 20 feet at a 90 degree angle to the sanitary line. The inspection direction is not exported with the observations, so the model has to be run separately for the downstream inspections and the upstream inspections.

### Steps

1. From WinCan - connect to the WinCan Map and export the Observation points
2. In ArcMap - load a shapefile of the sanitary line layer ``utl_sanitary_lines`` (export from original if need be)
3. Add the observation points to ArcMap
4. Add the following definition query to the observation points, adjusting the OBS_OPCODE to capture all taps if need be
```sql
"OBS_CLOCKP" <> 0 AND "OBS_OPCODE" IN ( 'TFA', 'TF', 'TBA')
```
5. Select the observations collected while running downstream
6. Run the Lateral_Creation model in the WinCan_Models.tbx toolbox against these observations, checking the ``Downstream`` checkbox
7. Do the same for the upstream observations, leaving the ``Downstream`` box unchecked
8. Join the created laterals to the observation points on the OBS_PK field
9. Copy over the DISTANCE field from the observations to the DISTANCE field in the laterals
10. Copy over the OBS_OBSERV field to the NOTES field in the laterals
11. Manually fill in the DIAMETER field
12. Manually fill in the Class field with either "Lateral" or "Storm"
13. Append the laterals to the master utl_sanitary_laterals feature in the Enterprise database (from QGIS)
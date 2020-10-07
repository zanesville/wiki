---
title: ArcGIS Desktop & PRO Quick Tips
tags: arcmap arcpro
subtitle: Duplicate values code, Sequential fields code, and more
category: test
---

## Check for Fields that have values that exist more than once in the same field

Use the "Summarize" tool, choose the field you want to check, using the "count" statistic, and a key field if needed, using "first"

## Check for Duplicate Values - DO NOT USE THIS METHOD

This method returns the first **duplicate**, not all values that exist more than once in the field.

```python
uniqueList = []
def isDuplicate(inValue):
  if inValue in uniqueList:
    return 1
  else:
    uniqueList.append(inValue)
    return 0
```

## Sequential Field Values
This is used to create the FIELDID for GIS assets. 

```python
rec=0 
def autoIncrement(): 
 global rec 
 pStart = 1  
 pInterval = 1 
 if (rec == 0):  
  rec = pStart  
 else:  
  rec += pInterval  
 return rec
```

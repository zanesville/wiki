---
title: ArcGIS Desktop & PRO Quick Tips & Tricks
tags: arcmap arcpro
subtitle: Duplicate values code, Sequential fields code, and more
---

## Check for Duplicate Values

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
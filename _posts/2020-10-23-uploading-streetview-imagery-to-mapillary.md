---
title: Uploading Streetview Imagery to Mapillary
tags:
- mapillary
category: tasks
---

1. Use render settings found in the capture playbook PDF for GoProFusion360.
2. 6K JPG No Parallax Framerate 2
3. Try the Mapillary uploader desktop program DOES NOT ALWAYS WORK
4. Use instead the Mapillary command line tool - using the folder of the rendered images, the username "zanesville" and the city organization name "coz"
5. https://github.com/mapillary/mapillary_tools
6. Use the tool from the same path as the exe file, or run it with the path to the exe file
7. Run it using cmd not powershell
```Python
mapillary_tools process_and_upload --import_path "Rendered/card_sd3_2761" --user_name "zanesville" --organization_username "coz"
```

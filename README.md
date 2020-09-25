# Geo-Sardine

Collection of spatial operation which i occasionally use written in python:
 - Interpolation with IDW (Inverse Distance Weighting) Shepard
 - Drape vector to raster
 - Spatial join between two vector

## Setup
install with pip
```pip install geosardine```

or anaconda
```conda install -c sahitono geosardine```

## How to use it

#### Drape and spatial join
```python
import geosardine as dine
import rasterio
import fiona

with rasterio.open("/home/user/data.tif") as raster, fiona.open("/home/user/data.shp") as vector:
    draped = dine.drape_geojson(vector, raster)
    joined = dine.spatial_join(vector, raster) 
```
#### IDW Interpolation
```python
import numpy as np
import geosardine as dine
xy = np.array([
        [106.8358,  -6.585 ],
        [106.6039,  -6.7226],
        [106.7589,  -6.4053],
        [106.9674,  -6.7092],
        [106.7956,  -6.5988]
])
values = np.array([132., 127.,  37.,  90., 182.])

"""
if epsg not provided, it will assume that coordinate is in wgs84 geographic
Find your epsg here https://epsg.io/
"""
interpolated = dine.interpolate.idw(xy, values, spatial_res=(0.01,0.01), epsg=4326)

# Save interpolation result to tiff
interpolated.save('idw.tif')

# shapefile or geojson can be used too
interp_file = dine.interpolate.idw("points.shp", spatial_res=(0.01,0.01), column_name="value")
interp_file.save() # output location is optional if file is the source

# The result array can be accessed like this
print(interpolated.array)
"""
[[ 88.63769859  86.24219616  83.60463194 ... 101.98185127 103.37001289
  104.54621272]
 [ 90.12053232  87.79279317  85.22030848 ... 103.77118852 105.01425289
  106.05302554]
 [ 91.82987695  89.60855271  87.14722258 ... 105.70090081 106.76928067
  107.64635337]
 ...
 [127.21214817 127.33208302 127.53878268 ...  97.80436475  94.96247196
   93.12113458]
 [127.11315081 127.18465002 127.33444124 ...  95.86455668  93.19212577
   91.51135399]
 [127.0435062  127.0827023  127.19214624 ...  94.80175756  92.30685734
   90.75707134]]
"""



```


## Geosardine CLI
You can use it through terminal or command prompt by calling **dine**

```
$ dine --help
Usage: dine [OPTIONS] COMMAND [ARGS]...

  GeoSardine CLI

Options:
  --help  Show this message and exit.

Commands:
  drape         Drape vector to raster to obtain height value
  info          Get supported format
  join-spatial  Join attribute by location
  idw           Create raster with Inverse Distance Weighting interpolation
```

### License
[![License](https://img.shields.io/badge/License-BSD%203--Clause-blue.svg)](https://opensource.org/licenses/BSD-3-Clause)

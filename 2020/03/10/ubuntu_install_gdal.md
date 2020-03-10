# Ubuntu18.04安装gdal2.2.3以及安装Python扩展包GDAL

### GDAL安装
```bash
# 1. update source list
sudo apt-get update

# 2. install gdal lib
sudo apt-get install gdal-bin

# 3. show gdal version info
gdalinfo --version
```

### Python扩展安装
```bash
# 1. install gdal dev lib
sudo apt-get install libgdal-dev

# 2. install python dev lib
sudo apt-get install python3.8-dev

# 3. export path info
export CPLUS_INCLUDE_PATH=/usr/include/gdal
export C_INCLUDE_PATH=/usr/include/gdal

# 4. pip install GDAL pkg
pip install GDAL==<GDAL VERSION FROM OGRINFO my is 2.2.3>
pip install GDAL==2.2.3
```

### Try it out
```python
# standard imports
import sys

# import OGR
from osgeo import ogr

# use OGR specific exceptions
ogr.UseExceptions()

# get the driver
driver = ogr.GetDriverByName("OpenFileGDB")

# opening the FileGDB
try:
    gdb = driver.Open(gdb_path, 0)
except Exception, e:
    print e
    sys.exit()

# list to store layers'names
featsClassList = []

# parsing layers by index
for featsClass_idx in range(gdb.GetLayerCount()):
    featsClass = gdb.GetLayerByIndex(featsClass_idx)
    featsClassList.append(featsClass.GetName())

# sorting
featsClassList.sort()

# printing
for featsClass in featsClassList:
    print featsClass

# clean close
del gdb
```
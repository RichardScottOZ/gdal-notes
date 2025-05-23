# gdal-notes
Notes on working with gdal apis and scripts

## translate
```
gdal_translate -a_srs EPSG:4326 -a_ullr -76.44533  -13.496806 -69.730871 -18.326707 "WMSServer (15).tif" wmsperuBB.tiff

gdal_translate -of JPEG -scale -co worldfile=yes input.tiff output.jpg	

gdal_translate "bgeofisica_sinal.tif Car2019_ASRichard.tif" "Car2019_ASRichardUInt16.tif" -ot UInt16

Brazil_DTM2 = r'E:\Exploracorn\brazil\s3\exploracorn\rasters\BRA_DTM2.tif'

#ulx uly lrx lry

from osgeo import gdal

ds = gdal.Open(Brazil_DTM)
ds = gdal.Translate(Brazil_DTM2, ds, projWin = [minx, maxy, maxx, miny])
ds = None

```

## calc
- calc to mask the Red Bank with a 0
```
gdal_calc.py -A "C:\users\rscott\OneDrive - OZ Minerals\Explorer Challenge\wmsperuBB.tiff" --outfile=tmp.tif --calc="A>1"  

gdal_calc.py -A wmsperuBB2.tiff --A_band=1 -B wmsperuBB2.tiff --B_band=3 --outfile=tmp.tif --calc="A>127,B>127"

gdal_calc.py -A wmsperuBB2.tiff --A_band=1 -B wmsperuBB2.tiff --B_band=3 --outfile=tmp.tif --calc="A>127*B>127"

gdal_calc.py -A wmsperuBB2.tiff --A_band=1 -B wmsperuBB2.tiff --B_band=3 --outfile=tmp.tif --calc="(A>127)*(B>127)"

gdal_calc.py -A ./hillshade.tmp.tif  --outfile=./opacity.tif --calc="255-A"
gdal_calc.py -A ./hillshade.tmp.tif  --outfile=./color_crop.tmp.tif   --calc="255*(A>220) +      A*(A<=220)"
# filter the opacity band, keep opacity of relevant shadows below limit
gdal_calc.py -A ./hillshade.tmp.tif  --outfile=./opacity_crop.tmp.tif  --calc="  1*(A>220) +(256-A)*(A<=220)"
	
	
gdal_calc.py -A wmsperuBB2.tiff --A_band=1 -B wmsperuBB2.tiff --B_band=3  -C wmsperuBB2.tiff --C_band=2 --outfile=tmp.tif --calc="(A>127)*(B>127)*(C<255)"

gdal_calc.py -A wmsperuBB2.tiff --A_band=1 -B wmsperuBB2.tiff --B_band=3  -C wmsperuBB2.tiff --C_band=2 --outfile=tmp.tif --calc="(A>127)*(B>108)*(C<255)"


```

## polygonize
```
gdal_polygonize.py "C:\users\rscott\OneDrive - OZ Minerals\Explorer Challenge\wmsperuBB.tiff" -f "ESRI Shapefile" wmsperuBB.shp

gdal_polygonize.py -mask tmp.tif wmsperuBB2.tiff -b 1 -q -f "ESRI Shapefile" junk1.shp test
```


## Geo Transform
```python
import gdal
import pandas as pd

def ix2xy(r,c,gt):
    '''Gets x,y from row and column'''
    x = gt[0] + r * gt[1]
    y = gt[3] + c * gt[5]
    return(x,y)
This little function gets the X/Y coordinates from the GeoTransform attribute which is a tuple with (xorigin, xres, 0, yorigin, 0, yres).
ds = gdal.Open('file.dat')
gt = ds.GetGeoTransform()

```

## merge
```
gdal_merge -o c:\temp\output_image.tif -q -v --optfile c:\temp\rasterlist.txt
gdal_merge -o c:\temp\output_image.tif -q -v --optfile c:\users\rscott\AlOHGroupComposition.txt

python gdal_merge.py -o C:\users\rscott\downloads\AusAEMxbf01merge.tif C:\Users\rscott\Downloads\AusAEM_Year1_Final_NT_Regional_Data\Final_NT_Regional_Lines\grid\emxbf\AusAEM_Year1_NT_Final_EMXbf01.ers C:\Users\rscott\Downloads\AusAEM_Year1_Final_QLD_Regional_Data\Final_QLD_Regional_Lines\grid\emxbf\AusAEM_Year1_QLD_Final_EMXbf01.ers

python c:\users\rscott\data\Scripts\gdal_merge -o c:\temp\AlOHGroup_Composition_.tif -q -v -init "0" --optfile c:\users\rscott\AlOHGroupComposition.txt
```

## warp
```
gdalwarp -t_srs EPSG:4326 input.tif output.tif

for %%N in (C:\Users\rscott\Downloads\*merge.ers) DO gdalwarp -t_srs EPSG:4326 %%N C:\Users\rscott\Downloads\temp\%%~nN.ers

for %N in (C:\Users\rscott\Downloads\*merge.ers) DO gdalwarp -t_srs EPSG:4326 %N C:\Users\rscott\Downloads\temp\%~nN.ers

```

## Errors
C:\Users\rnmsc\OneDrive\GIT\ML-Framework\geoscience-ml>python tests\test_cnn_model.py
Traceback (most recent call last):
  File "C:\Users\rnmsc\OneDrive\GIT\ML-Framework\geoscience-ml\tests\test_cnn_model.py", line 9, in <module>
    from src.utils.dummy_data import DummyDataGenerator
  File "c:\users\rnmsc\onedrive\git\ml-framework\geoscience-ml\src\utils\__init__.py", line 1, in <module>
    from .explainability import ModelExplainer
  File "c:\users\rnmsc\onedrive\git\ml-framework\geoscience-ml\src\utils\explainability.py", line 13, in <module>
    import rasterio
  File "C:\Users\rnmsc\miniconda3\envs\pangeo\lib\site-packages\rasterio\__init__.py", line 28, in <module>
    from rasterio._version import gdal_version, get_geos_version, get_proj_version
ImportError: DLL load failed while importing _version: The specified procedure could not be found.

Don't mix pip install rasterio with conda GDAL

Never install GDAL from default channel - use conda-forge

Avoid Python 3.11+ (stick with 3.9/3.10 for better compatibility)


conda create -n geo_env -c conda-forge python=3.9 rasterio=1.3.9 gdal=3.7.2


# gdal-notes
Notes on working with gdal apis and scripts

## translate
```
gdal_translate -a_srs EPSG:4326 -a_ullr -76.44533  -13.496806 -69.730871 -18.326707 "WMSServer (15).tif" wmsperuBB.tiff

gdal_translate -of JPEG -scale -co worldfile=yes input.tiff output.jpg	

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
```

## warp
```
gdalwarp -t_srs EPSG:4326 input.tif output.tif
```
# gdal-notes
Notes on working with gdal apis and scripts

## translate
```
gdal_translate -a_srs EPSG:4326 -a_ullr -76.44533  -13.496806 -69.730871 -18.326707 "WMSServer (15).tif" wmsperuBB.tiff
```

## calc
- calc to mask the Red Bank with a 0
```
python gdal_calc.py -A "C:\users\rscott\OneDrive - OZ Minerals\Explorer Challenge\wmsperuBB.tiff" --outfile=tmp.tif --calc="A>1"  

gdal_calc.py -A wmsperuBB2.tiff --A_band=1 -B wmsperuBB2.tiff --B_band=3 --outfile=tmp.tif --calc="A>127,B>127"

gdal_calc.py -A wmsperuBB2.tiff --A_band=1 -B wmsperuBB2.tiff --B_band=3 --outfile=tmp.tif --calc="A>127*B>127"

gdal_calc.py -A wmsperuBB2.tiff --A_band=1 -B wmsperuBB2.tiff --B_band=3 --outfile=tmp.tif --calc="(A>127)*(B>127)"

```

## polygonize
```
python gdal_polygonize.py "C:\users\rscott\OneDrive - OZ Minerals\Explorer Challenge\wmsperuBB.tiff" -f "ESRI Shapefile" wmsperuBB.shp
``

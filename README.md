# gdal-notes
Notes on working with gdal apis and scripts

# translate
```
gdal_translate -a_srs EPSG:4326 -a_ullr -76.44533  -13.496806 -69.730871 -18.326707 "WMSServer (15).tif" wmsperuBB.tiff
```

# calc
- calc to mask the Red Bank with a 0
```
python gdal_calc.py -A "C:\users\rscott\OneDrive - OZ Minerals\Explorer Challenge\wmsperuBB.tiff" --outfile=tmp.tif --calc="A>1"  
```

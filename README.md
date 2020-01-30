# Descarga del portal de GBIF de los registros de presencia de especies de _Chordata_ presentes en la GAM de Costa Rica
Este repositorio contiene el procedimiento para la descarga, del portal de la [Infraestructura Mundial de Información en Biodiversidad (GBIF)](https://www.gbif.org/), de los registros de presencia de especies del filo _Chordata_ (vertebrados) presentes en la gran área metropolitana (GAM) de Costa Rica.

Para seguir el procedimiento, es necesaria la biblioteca [Geospatial Data Abstraction Library (GDAL)](https://gdal.org/) para la lectura y escritura de datos geoespaciales. Se recomieda utilizar la versión incluída en la plataforma de ciencia de datos [Anaconda](https://www.anaconda.com/).

1. Clonación del repositorio:
```terminal
$ git clone https://github.com/catie-ume/descarga-gbif-chordata-cr-gam.git
$ cd descarga-gbif-chordata-cr-gam
```
2. Conversión a WGS84:
```terminal
$ ogr2ogr -t_srs EPSG:4326 datos/Limite_GAM_Plan82_2013_08_13_wgs84.shp datos/Limite_GAM_Plan82_2013_08_13.shp
```

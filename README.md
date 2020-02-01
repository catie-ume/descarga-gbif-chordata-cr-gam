# Descarga del portal de GBIF de los registros de presencia de especies de _Mammalia_, _Amphibia_, _Reptilia_, _Aves_ y _Plantae_ presentes en la GAM de Costa Rica
Este repositorio contiene el procedimiento para la descarga, del portal de la [Infraestructura Mundial de Información en Biodiversidad (GBIF)](https://www.gbif.org/), de los registros de presencia de especies de las clases _Mammalia_ (mamíferos), _Amphibia_ (anfibios), _Reptilia_ (reptiles) y _Aves_ (aves); y del reino _Plantae_ (plantas) presentes en la gran área metropolitana (GAM) de Costa Rica.

Para realizar el procedimiento, es necesaria la biblioteca [Geospatial Data Abstraction Library (GDAL)](https://gdal.org/) para la lectura y escritura de datos geoespaciales, de la cual se utilizarán los utilitarios de línea de comandos [ogr2ogr](https://gdal.org/programs/ogr2ogr.html) y [ogrinfo](https://gdal.org/programs/ogrinfo.html). Se sugiere utilizar la versión de GDAL incluída en la plataforma de ciencia de datos [Anaconda](https://www.anaconda.com/).

La consulta al portal de GBIF recibe como parámetros principales:
* Los nombres científicos de los taxones.
* El polígono de la GAM.

El polígono de la GAM proviene un _shapefile_ facilitado por el Ministerio de Vivienda y Asentamientos Humanos (Mivah), el cual se incluye en este repositorio, en el subdirectorio /datos. Para utilizarse en el portal de GBIF, la geometría debe expresarse en formato [GeoJSON](https://geojson.org/) o [Well Known Text (WKT)](https://en.wikipedia.org/wiki/Well-known_text_representation_of_geometry) en la proyección WGS84. Además, no debe ser demasiado compleja (i.e. con demasiados vértices). A continuación, se enumeran los pasos para realizar la conversión de la geometría a WKT en WGS84, simplificarla y utilizarla en una consulta al portal de GBIF.

**1. Clonación del repositorio**
```terminal
$ git clone https://github.com/catie-ume/descarga-gbif-chordata-cr-gam.git
$ cd descarga-gbif-chordata-cr-gam
```
**2. Conversión de la geometría a la proyección WGS84**
```terminal
$ ogr2ogr \
    -t_srs EPSG:4326 \
    datos/Limite_GAM_Plan82_2013_08_13_wgs84.shp \
    datos/Limite_GAM_Plan82_2013_08_13.shp
```
**3. Simplificación de la geometría**
```terminal
$ ogr2ogr \
    -t_srs EPSG:4326 \
    -simplify 0.01 \
    datos/Limite_GAM_Plan82_2013_08_13_wgs84_simp01.shp \
    datos/Limite_GAM_Plan82_2013_08_13_wgs84.shp
```
**4. Despliegue de la geometría**
```terminal
$ ogrinfo 
    -al 
    datos/Limite_GAM_Plan82_2013_08_13_wgs84_simp01.shp
```
Resultado:
```terminal
POLYGON ((-83.9779609469256 9.74701527840667,-83.9998166303865 9.80913368562122,-84.0686620894165 9.80725466392944,-84.1935596879496 9.88787413069186,-84.2786338246922 9.86128678448095,-84.2983634693345 9.87681562578536,-84.2780907875851 9.88451379870899,-84.2864606620801 9.89607347178097,-84.3265918056867 9.91666122742282,-84.3891954885131 9.9361277204225,-84.4615472332293 9.92131212990945,-84.4576196690076 9.99576802387299,-84.4345934085902 9.97249041262442,-84.3926039583673 9.99704617261992,-84.3104759871981 9.99845665668619,-84.2356302991999 10.1592207628453,-84.1636867837922 10.159191056575,-84.1012896021423 10.1310142029792,-84.0771277159251 10.1171581708717,-84.0797267286753 10.0870406949915,-84.0313822864563 10.0570034872918,-83.9752452875658 10.0664556474202,-83.9891291689868 10.0355104931829,-83.8965867447576 9.96784944612683,-83.8380463802749 9.97139115255637,-83.8127308664787 10.0169910177019,-83.7608466846179 9.94849600004722,-83.7779338694645 9.94262495190389,-83.7620395912059 9.91191587763317,-83.7786413930152 9.8958221822262,-83.7612008894899 9.86244533273949,-83.7809967818276 9.85581260717074,-83.7609354609194 9.84126848638818,-83.7859460026121 9.77624969753637,-83.7666118555309 9.76800592548316,-83.7666255310591 9.74285452978233,-83.9779609469256 9.74701527840667))
```
**5. Consulta al portal de GBIF**
Parámetros para la interfaz de [búsqueda de registros de presencia](https://www.gbif.org/occurrence/search):
* Scientific name: Mammalia (class), Amphibia (class), Reptilia (class), Aves (class) y Plantae (kingdom)
* Location:
  * GeoJSON: El polígono en formato WKT desplegado en el paso 4.
  
El resultado de la consulta está disponible en [https://www.gbif.org/occurrence/download/0002756-200127171203522](https://www.gbif.org/occurrence/download/0002756-200127171203522).

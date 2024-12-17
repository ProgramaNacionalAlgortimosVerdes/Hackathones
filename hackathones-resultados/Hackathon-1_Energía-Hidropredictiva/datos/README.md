# Datos a predecir - *la variable target*

Los datos de la variable target (caudal), que aparecen en este reto han sido transformados por cuestiones de confidencialidad. Se ha utilizado el z-score para ello, lo que puede mostrar resultados físicamente incorrectos (datos de caudal negativos).

### Mediciones del caudal de los emplazamientos:

Como hemos mencionado previamente, los datos a predecir son mediciones del caudal diario medio de 5 emplazamientos en la cuenca del Duero. Contamos con mediciones diarias para los 5 emplazamientos desde el 01/01/2021 al 31/03/2024

#### Sets de Train y Test 


Dada la naturaleza de Kaggle y las metodologías de testing de los modelos predictivos de series temporales, hemos escogido el test set de manera muy cuidadosa. Kaggle no permite a los participantes acceder a los datos de test y las metodologías de testing y validación de series temporales suelen requerir tener acceso a estos para que los modelos cuenten con la variable a predecir rezagada *(lags)*. Por este motivo, hemos escogido las fechas del *test set* tal que podamos maximizar la cantidad de variables rezagadas a las que los participantes pueden acceder para entrenar sus modelos, y así perdamos la menor cantidad de datos y rigurosidad al medir el rendimiento de los modelos. 

Para el *test set* hemos extraído mediciones del caudal de **dos días consecutivos** de las 26 fechas del último año que maximizan la variación entre el caudal de la fecha previa a la predicción y las dos fechas a predecir. **Se mantiene al menos 20 días de separación entre todas las fechas consecutivas extraídas**. Las fechas en las que hicimos las extracciones son las mismas para  los cinco emplazamientos.

 Estas son:



<table>
  <tr>
    <th>Índice Test Set</th>
    <th>Fecha inicio</th>
    <th>Fecha fin</th>
  </tr>
  <tr>
    <td>1</td>
    <td>2023-04-12</td>
    <td>2023-04-13</td>
  </tr>
  <tr>
    <td>2</td>
    <td>2023-05-03</td>
    <td>2023-05-04</td>
  </tr>
  <tr>
    <td>3</td>
    <td>2023-06-08</td>
    <td>2023-06-09</td>
  </tr>
  <tr>
    <td>4</td>
    <td>2023-07-05</td>
    <td>2023-07-06</td>
  </tr>
  <tr>
    <td>5</td>
    <td>2023-07-31</td>
    <td>2023-08-01</td>
  </tr>
  <tr>
    <td>6</td>
    <td>2023-09-05</td>
    <td>2023-09-06</td>
  </tr>
  <tr>
    <td>7</td>
    <td>2023-09-26</td>
    <td>2023-09-27</td>
  </tr>
  <tr>
    <td>8</td>
    <td>2023-11-03</td>
    <td>2023-11-04</td>
  </tr>
  <tr>
    <td>9</td>
    <td>2023-12-03</td>
    <td>2023-12-04</td>
  </tr>
  <tr>
    <td>10</td>
    <td>2023-12-25</td>
    <td>2023-12-26</td>
  </tr>
  <tr>
    <td>11</td>
    <td>2024-01-18</td>
    <td>2024-01-19</td>
  </tr>
  <tr>
    <td>12</td>
    <td>2024-02-12</td>
    <td>2024-02-13</td>
  </tr>
  <tr>
    <td>13</td>
    <td>2024-03-11</td>
    <td>2024-03-12</td>
  </tr>
</table>




El gráfico de las 5 series temporales de los puntos de train (después de la extracción de las mediciones de test) se ve de la siguiente manera:

![](https://www.googleapis.com/download/storage/v1/b/kaggle-user-content/o/inbox%2F22590444%2F383789fe82a2af5ff8c6548128ad64c6%2Fnewplot%20(4).png?generation=1726765780538273&alt=media)




En el gráfico las bandas muestran las fechas en las que extrajimos los datos de test. Estos deberán ser predichos como forecasts a 24 y a 48 horas con las mediciones del caudal de los aforos y las predicciones de otras variables meteorológicas a 48 horas del GFS. 

Estos datos se encuentran en el archivo **target_emplazamientos_train.csv**.

--------

# Datos para usar como variables *predictivas*

#### Mediciones del caudal de los aforos

También contamos con mediciones del caudal diario medio de 5 aforos en la cuenca del Duero en el mismo plazo temporal. El caudal en estos aforos está correlacionado con el caudal de los emplazamientos, así que los participantes podrán considerar incluirlos en sus modelos.

Estos datos se encuentran en los archivos **predictor_aforos.csv**.

#### Global Forecasting System (GFS)

Por otro lado, contamos con un subconjunto de salidas de simulaciones del Global Forecast System (GFS) a .25 grados de resolución, un modelo numérico de predicción meteorológica operado por la Administración Nacional Oceánica y Atmosférica (NOAA) de Estados Unidos. Este modelo global es fundamental para la predicción meteorológica, ofreciendo datos detallados sobre una amplia gama de condiciones atmosféricas y del suelo que pueden impactar significativamente el caudal de los emplazamientos.

El GFS proporciona pronósticos globales de alta resolución que abarcan diversos parámetros atmosféricos y terrestres. Estos datos son esenciales no solo para la predicción meteorológica y la vigilancia de fenómenos climáticos extremos, sino también para investigaciones científicas y aplicaciones prácticas en gestión de recursos naturales. Las variables proporcionadas permiten modelar con precisión las condiciones que afectan el flujo de agua en los emplazamientos.

##### Variables 



A continuación, se describen las variables del GFS incluidas en este desafío:

<div style=“text-align: center;”> <table> <thead> <tr> <th>Etiqueta GFS</th> <th>Etiqueta CSV</th> <th>Unidad</th> <th>Descripción</th> </tr> </thead> <tbody> <tr> <td>APCP</td> <td>APCP_0_SFC</td> <td>kg/m²</td> <td>Precipitación Total acumulada en las últimas 6 horas. Esta variable mide la cantidad total de precipitación que ha caído en el área de interés durante el período especificado.</td> </tr> <tr> <td>ACPCP</td> <td>ACPCP_0_SFC</td> <td>kg/m²</td> <td>Precipitación Convectiva acumulada en las últimas 6 horas. Refleja la cantidad de precipitación generada por procesos convectivos en el mismo período.</td> </tr> <tr> <td>WATR</td> <td>WATR_0_SFC</td> <td>kg/m²</td> <td>Escorrentía de Agua acumulada en las últimas 6 horas. Representa la cantidad de agua que ha escurrido en el área de estudio durante el período especificado.</td> </tr> <tr> <td>WEASD</td> <td>WEASD_0_SFC</td> <td>kg/m²</td> <td>Equivalente de Agua de la Profundidad de Nieve Acumulada. Indica la cantidad de agua contenida en la nieve acumulada.</td> </tr> <tr> <td>SNOD</td> <td>SNOD_0_SFC</td> <td>m</td> <td>Profundidad de Nieve. Mide la profundidad de la capa de nieve en el área de interés.</td> </tr> <tr> <td>DPT</td> <td>DPT_2_HTGL</td> <td>K</td> <td>Temperatura del Punto de Rocío. La temperatura a la cual el aire se satura con humedad, causando la formación de rocío.</td> </tr> <tr> <td>var255: 0-10 cm down</td> <td>var255 of table 3 of center 7_0_10_DBLY</td> <td>Proporción</td> <td>Humedad Volumétrica del Suelo (no Congelada) a 0-0.1 m debajo de la superficie.</td> </tr> <tr> <td>var255: 10-40 cm down</td> <td>var255 of table 3 of center 7_10_40_DBLY</td> <td>Proporción</td> <td>Humedad Volumétrica del Suelo (no Congelada) a 0.1-0.4 m debajo de la superficie.</td> </tr> <tr> <td>var255: 40-100 cm down</td> <td>var255 of table 3 of center 7_40_100_DBLY</td> <td>Proporción</td> <td>Humedad Volumétrica del Suelo (no Congelada) a 0.4-1 m debajo de la superficie.</td> </tr> <tr> <td>var255: 100-200 cm down</td> <td>var255 of table 3 of center 7_100_200_DBLY</td> <td>Proporción</td> <td>Humedad Volumétrica del Suelo (no Congelada) a 1-2 m debajo de la superficie.</td> </tr> <tr> <td>TMP</td> <td>TMP_2_HTGL</td> <td>K</td> <td>Temperatura. La temperatura del aire a una altura de 2 metros sobre el suelo.</td> </tr> <tr> <td>PEVPR</td> <td>PEVPR_0_SFC</td> <td>W/m²</td> <td>Tasa de Evaporación Potencial. La cantidad de energía disponible para la evaporación en el área de estudio.</td> </tr> <tr> <td>DSWRF</td> <td>DSWRF_0_SFC</td> <td>W/m²</td> <td>Flujo de Radiación Solar Descendente acumulado en las últimas 6 horas. Indica la cantidad total de radiación solar que ha llegado a la superficie.</td> </tr> <tr> <td>DLWRF</td> <td>DLWRF_0_SFC</td> <td>W/m²</td> <td>Flujo de Radiación Infrarroja Descendente acumulado en las últimas 6 horas. Mide la radiación infrarroja que llega a la superficie.</td> </tr> <tr> <td>USWRF</td> <td>USWRF_0_SFC</td> <td>W/m²</td> <td>Flujo de Radiación Solar Ascendente acumulado en las últimas 6 horas. Representa la radiación solar que ha sido reflejada hacia arriba desde la superficie.</td> </tr> <tr> <td>ULWRF</td> <td>ULWRF_0_SFC</td> <td>W/m²</td> <td>Flujo de Radiación Infrarroja Ascendente acumulado en las últimas 6 horas. Mide la radiación infrarroja emitida desde la superficie hacia el espacio.</td> </tr> <tr> <td>UGRD</td> <td>UGRD_10_HTGL</td> <td>m/s</td> <td>Componente U del Viento a 10 metros sobre el suelo. Mide la velocidad del viento en la dirección este-oeste.</td> </tr> <tr> <td>VGRD</td> <td>VGRD_10_HTGL</td> <td>m/s</td> <td>Componente V del Viento a 10 metros sobre el suelo. Mide la velocidad del viento en la dirección norte-sur.</td> </tr> <tr> <td>PRES</td> <td>PRES_0_SFC</td> <td>Pa</td> <td>Presión. La presión atmosférica en la superficie del suelo.</td> </tr> <tr> <td>VIS</td> <td>VIS_0_SFC</td> <td>m</td> <td>Visibilidad. La distancia máxima a la que un objeto o punto puede ser claramente visto.</td> </tr> <tr> <td>LHTFL</td> <td>LHTFL_0_SFC</td> <td>W/m²</td> <td>Flujo Neto de Calor Latente acumulado en las últimas 6 horas. La cantidad de energía transportada en forma de calor latente durante el período.</td> </tr> <tr> <td>SHTFL</td> <td>SHTFL_0_SFC</td> <td>W/m²</td> <td>Flujo Neto de Calor Sensible acumulado en las últimas 6 horas. La cantidad de energía transportada en forma de calor sensible durante el período.</td> </tr> <tr> <td>FLDCP</td> <td>FLDCP_0_SFC</td> <td>Fracción</td> <td>Capacidad de Campo. La máxima cantidad de agua que el suelo puede retener contra la gravedad.</td> </tr> <tr> <td>HPBL</td> <td>HPBL_0_SFC</td> <td>m</td> <td>Altura de la Capa Fronteriza Planetaria. La altura de la capa de la atmósfera que se mezcla debido a la turbulencia.</td> </tr> </tbody> </table> </div>

##### Estructura dataset

Contamos con cuatro variables más que determinan la estructura del data set; estas pueden ser consideradas como índices de las observaciones.

- La variable `time` señala el día en el cual se ha llevado acabo la predicción. Parte de la importancia de esta variable es que nos indica el plazo temporal de las mediciones meteorológicas que se han incluido en la predicción. Contamos con observaciones de la variable `time` en el mismo plazo temporal que las mediciones de los caudales.

- La variable  `valid_time` señala para cuando es valida la predicción.  Cada valor de `time` está asociado a 8 valores de  `valid_time`. Es decir, contamos con una predicción de las variables meteorológicas cada 6 horas, hasta llegar a la predicción a 48 horas. Se puede ver esta estructura en tabla abajo.

- Finalmente, contamos con las variables de  `latitude` y  `longitude`, las cuales indican en que localización geográfica es válida la predicción. Por cada predicción asociada a un valor de `valid_time` tenemos observaciones de latitudes y longitudes que cubren la península ibérica (como se puede observar en el mapa de la sección de **Overview**).

Por ejemplo, los datos asociados a `time` 2023-09-05 y a la `latitude` 39.75 y la `longitude` -2.25 tienen la siguiente estructura:

    time, valid_time, latitude, longitude,APCP_0_SFC,...,HPBL_0_SFC
    2023-09-05 00:00:00,2023-09-05 00:06:00,39.75,-2.25,0,...,0
    2023-09-05 00:00:00,2023-09-05 00:12:00,39.75,-2.25,0,...,0
    2023-09-05 00:00:00,2023-09-05 00:18:00,39.75,-2.25,0,...,0
    2023-09-05 00:00:00,2023-10-05 00:00:00,39.75,-2.25,0,...,0
    2023-09-05 00:00:00,2023-10-05 00:06:00,39.75,-2.25,0,...,0
    2023-09-05 00:00:00,2023-10-05 00:12:00,39.75,-2.25,0,...,0
    2023-09-05 00:00:00,2023-10-05 00:18:00,39.75,-2.25,0,...,0
    2023-09-05 00:00:00,2023-10-06 00:00:00,39.75,-2.25,0,...,0

Esto significa que contamos con predicciones hechas el día 2023-09-05 para las siguientes 48 horas, en intervalos de 6 horas, para la latitud 39.75 y la longitud -2.25.

Estos datos se encuentran en el archivo **gfs.csv**.

---

### Notebook Demonstración

En el [notebook de demonstración](https://www.kaggle.com/code/algoritmosverdespnav/notebook-demo-hidropredictiva), se puede ver un ejemplo de como entrenar un modelo utilizando estos datos.



--------------

## Otras fuentes de datos

Los participantes pueden utilizar datos externos siempre que sean de acceso público y se cite la fuente. Sin embargo, no está permitido que los participantes usen otro modelo meteorológico a parte del provisto.


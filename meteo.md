---
layout: page
permalink: /meteo/
title: Meteorología
description: "Información general sobre meteo"
tags: [meteo, prediccion]
image:
  feature: texture-feature-02.jpg
  credit: José Manuel Prieto Blázquez
  creditlink: http://jmprietob.github.io
---

## Predicción a partir del 22 de mayo

#### Precipitación
La animación se compone de un mapa de nubosidad (nubes medias y bajas), de la cantidad de precipitación y nieve (naranja).  
<figure>
	<img src="/images/precipitacion.gif">
</figure>

#### Viento
Mapa base de velocidad (en metros por segundo) con la dirección del viento representada en líneas de flujo.
<figure>
	<img src="/images/viento.gif">
</figure>

#### Un punto concreto
También he actualizado los datos de la [aplicación meteo](https://jmprietob.shinyapps.io/meteo/) donde con los datos de latitud y longitud de un punto se extraen datos de precipitación, temperatura y cota de nieve(datos dentro de España).


Un [modelo númerico de predicción meteorológica](http://es.wikipedia.org/wiki/Modelo_num%C3%A9rico_de_predicci%C3%B3n_meteorol%C3%B3gica) realiza, partiendo de un estado inicial con una atmósfera determinadas, una simulación de la evolución atmosférica a través de métodos numéricos.

## Información general

#### Sobre el modelo
El modelo utilizado es el [WRF](http://www.wrf-model.org), que se basa en el modelo [GFS](http://es.wikipedia.org/wiki/Global_Forecast_System) (un modelo más global). El modelo WRF cubre casi toda Europa y el Mediterraneo. Las condiciones iniciales y los límites vienen del modelo GFS. Las previsiones incluyen velocidad, dirección y rafagas de viento, temperatura, nubosidad total y precipitaciones. 

#### Sobre los datos
El servidor tiene datos de modelos de predicción con diferente tamaño de cuadrícula (4, 12, 36 km) y en diferente formato (wms, grib, netcdf). El servidor de datos utilizado es el de [Meteogalicia]( http://www.meteogalicia.es/web/modelos/threddsIndex.action?request_locale=es).

#### Animación de modelo
Para las animaciones utilizo el software [IDV](http://www.unidata.ucar.edu), que es gratuito y tiene licencia GPL, aunque hay que registrarse para su descarga.

#### Webs sobre meteorología
A parte de la [AEMET](http://www.aemet.es), existen muchas páginas y software donde uno puede investigar. Aquí os dejo algunos enlaces:

+ [ZyGRIB](http://www.zygrib.org/): software opensource donde puedes descargarte desde el mismo programa los datos del modelo GFS del NOAA para el área del muldo que quieras.

+ [Santander Meteorology Group](http://www.meteo.unican.es/imeteo/home): datos del modelo WFS, mapas.

+ [Grupo de Física de la Atmósfera de la Universidad de León](http://gfa.unileon.es/?q=es/node/35): más centrado en Castilla León también tienen datos de observación y predicción.

+ [MeteoCAT](http://www.meteo.cat/servmet/index.html): para Cataluña.

+ [euskalmet](http://www.euskalmet.euskadi.net/s07-5853x/es/meteorologia/home.apl?e=5): la agencia vasca de meteorología.
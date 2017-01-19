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

## Mapa meteorológico de [Windytv](https://www.windytv.com)

<iframe width="650" height="450" src="https://embed.windytv.com/embed2.html?lat=40.145&lon=-3.955&zoom=5&level=surface&overlay=rain&menu=&message=&marker=&forecast=12&calendar=now&location=coordinates&type=map&actualGrid=%5Bobject%20HTMLSelectElement%5D&metricWind=km%2Fh&metricTemp=%C2%B0C" frameborder="0"></iframe> 

## Información general
Actualizada la [aplicación meteo](https://jmprietob.shinyapps.io/meteo/) donde introduciendo una localidad se extraen datos de precipitación, temperatura y cota de nieve(datos para la España peninsular).

#### Sobre el modelo
Un [modelo numérico de predicción meteorológica](http://es.wikipedia.org/wiki/Modelo_num%C3%A9rico_de_predicci%C3%B3n_meteorol%C3%B3gica) realiza, partiendo de un estado inicial con una atmósfera determinadas, una simulación de la evolución atmosférica a través de métodos numéricos.
El modelo utilizado es el [WRF](http://www.wrf-model.org), que se basa en el modelo [GFS](http://es.wikipedia.org/wiki/Global_Forecast_System) (un modelo más global). El modelo WRF cubre casi toda Europa y el Mediterraneo. Las condiciones iniciales y los límites vienen del modelo GFS. Las previsiones incluyen velocidad, dirección y rafagas de viento, temperatura, nubosidad total y precipitaciones. 

#### Sobre los datos
El servidor tiene datos de modelos de predicción con diferente tamaño de cuadrícula (4, 12, 36 km) y en diferente formato (wms, grib, netcdf). El servidor de datos utilizado es el de [Meteogalicia]( http://www.meteogalicia.es/web/modelos/threddsIndex.action?request_locale=es).

## Webs sobre meteorología
A parte de la [AEMET](http://www.aemet.es), existen muchas páginas y software donde uno puede investigar. Aquí os dejo algunos enlaces:

+ [ZyGRIB](http://www.zygrib.org/): software opensource donde puedes descargarte desde el mismo programa los datos del modelo GFS del NOAA para el área del muldo que quieras.

+ [Santander Meteorology Group](http://www.meteo.unican.es/imeteo/home): datos del modelo WFS, mapas.

+ [Grupo de Física de la Atmósfera de la Universidad de León](http://gfa.unileon.es/?q=es/node/35): más centrado en Castilla León también tienen datos de observación y predicción.

+ [MeteoCAT](http://www.meteo.cat/servmet/index.html): para Cataluña.

+ [euskalmet](http://www.euskalmet.euskadi.net/s07-5853x/es/meteorologia/home.apl?e=5): la agencia vasca de meteorología.
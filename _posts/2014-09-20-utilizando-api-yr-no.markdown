---
layout: post
title:  "Utilizando la api de yr.no"
description: "Meteorology prediction"
date:   2014-09-20 10:00:00
categories: prediccion
tags: [predicción, meteorología, R, Shiny, API]
image:
  feature: texture-feature-02.jpg
  credit: José Manuel Prieto Blázquez
  creditlink: http://jmprietob.github.io
comments: true
share: true
---

Después de este parón vacacional y de la vuelta al trabajo retomo este *bloc de notas*.

En esta ocasión es sobre la utilización de una **api** pública de datos y creación de una aplicación **shiny** para visualización de los mismos.

La **api** es del **Norwegian Meteorological Institute** [^1]. Esta es una de las más populares y ofrece datos de meteorológicos para unos 7 millones de lugares alrededor del mundo (eso dicen ellos y no los he contado).

La **api** devuelve un **xml** con información referente a nubosidad, precipitación, humedad, velocidad del viento, dirección del viento, temperatura, para unas coordenadas dadas.

La documentación referente a la api pública se puede encontrar aquí [^2]. El servicio que utilizo es **Locationforecast**.

El códígo se puede encontrar en mi repositorio de [github](https://github.com/jmprietob/rapps/tree/master/eltiempo) y la aplicación funcional, en [shinyapps.io](https://jmprietob.shinyapps.io/eltiempo/).

[^1]: [http://www.met.no/](http://www.met.no/)
[^2]: [http://api.yr.no/weatherapi/documentation](http://api.yr.no/weatherapi/documentation)


---
layout: post
title:  "Aplicación Meteo con geocodificación"
description: "R Development."
date:   2014-06-02 10:00:00
categories: programacion
tags: [programación, R, predicción]
image:
  feature: texture-feature-02.jpg
  credit: José Manuel Prieto Blázquez
  creditlink: http://jmprietob.github.io
comments: true
share: true
---

He actualizado el código de la aplicación shiny sobre meteorología. Ahora utiliza el servicio de geocodificación del package [ggmap](http://cran.r-project.org/web/packages/ggmap/index.html) para extraer las coordenadas dada una localización. Con esa coordenadas extrae los datos de modelo meteorológico para esa localización y genera un gráfico.

Su uso es muy sencillo:
{% highlight r %}
	#cargar package
	library(ggmap)
	geocode('Madrid')
	       lon      lat
	1 -3.70379 40.41678
{% endhighlight %}

### La aplicación
El código y los datos de prueba se pueden descargar de mi
[repositorio de Github](https://github.com/jmprietob/rapps/tree/master/meteo). Puedes obtener la predicción horaria para tu localización en [Aplicación meteo](https://jmprietob.shinyapps.io/meteo/).

### Meteorología
También he actualizado las animaciones de precipitación y viento para los próximos días en la sección [meteorología](http://jmprietob.github.io/meteo/) del blog.



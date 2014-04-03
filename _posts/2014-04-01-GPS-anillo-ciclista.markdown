---
layout: post
title:  "GPS Anillo Ciclista de Madrid"
description: "GPS Anillo Ciclista de Madrid."
date:   2014-04-01 13:00:00
categories: programacion
tags: [GPS, R, programación]
image:
  feature: texture-feature-02.jpg
  credit: José Manuel Prieto Blázquez
  creditlink: http://jmprietob.github.io
comments: true
share: true
---

Hace unos días salí a probar la nueva bici con el soporte para el GPS y me di un paseo por el [Anillo Ciclista de Madrid](http://www.anilloverdeciclista.es/). Grabé el recorrido (track) en el GPS para posteriormente procesarlo, mapa de la ruta, perfiles, estadísticas.

### GPS y software de escritorio

Tengo un Garmin Etrex 20 y como software de escritorio utilizo Google Earth y [QLandkarte GT](http://www.qlandkarte.org/), que es Open Source y tiene versiones para Windows, OS X, Linux.

> Mapa y perfil del [QLandkarte GT](http://www.qlandkarte.org/):

<figure>
	<img src="/images/anillo.png">
</figure>

<figure>
	<img src="/images/perfilanillo.png">
</figure>

### R

Había utilizado R para leer GPX de puntos (waypoints) con la librería XML (GPX es un XML) e introducirlos en una base de datos de Acces a través de RODBC.

Investigando, hay varias maneras de leer `tracks` a través de diferentes librerías `Rgdal`, `maptools`, `plotkml`. 

En este caso he utilizado `plotKML` que tiene una función `readGPX` que nos devuelve un objeto `list` con diferente información del archivo `GPX` (tracks, waypoints, routes, bounds, metadata).

El siguiente script lee el archivo GPX, calcula distancias parciales, acumuladas y velocidad media, genera unos mapas con el recorrido y unos perfiles longitudinales.

{% highlight r %}

library(plotKML)
library(ggmap)
library(ggplot2)
library(gridExtra)
gpx <- readGPX("Track.gpx", metadata = TRUE, bounds = TRUE, 
        waypoints = FALSE, tracks = TRUE, routes = FALSE)
# readGPX devuelve una lista de objetos.
# Nos quedamos con el dataframe
gpxDF<-data.frame(gpx$tracks[[1]])
names(gpxDF) <- c('lon', 'lat', 'ele', 'time')
# Los campos ele y time son chr.
# Cambio a POSIX y number
gpxDF$time <- as.POSIXct(gpxDF$time,format="%Y-%m-%dT%H:%M:%S")
gpxDF$ele <- as.numeric(gpxDF$ele)
# Creamos las columnas para las distancias entre puntos consecutivos y acumulada
gpxDF$dist=0;
gpxDF$disAcu=0;
# Columna de velocidad media entre dos puntos consecutivos
gpxDF$vm=0;

# Utilizamos la fórmula de Haversine para calcular la distancia entre dos puntos consecutivos de lat-lon
# http://es.wikipedia.org/wiki/F%C3%B3rmula_del_Haversine
# Calculo de la distancia 2D
# Función adaptada de http://lwlss.net/GarminReports/GarminFunctions.R
radTierra=6371000
dist<-function(aLong,aLat,bLong,bLat){
  dLat=2*pi*(bLat-aLat)/360.0
  dLon=2*pi*(bLong-aLong)/360.0
  a=(sin(dLat/2))^2+cos(2*pi*aLat/360)*cos(2*pi*bLat/360)*(sin(dLon/2)^2)
  return(radTierra*2*atan2(sqrt(a),sqrt(1-a)))
}

# Calculo de distancias parciales y velocidad media entre puntos
for (x in 1:(nrow(gpxDF))) {
  if (x == 1){
    gpxDF$dist[1] <- 0
  }
  else {
    gpxDF$dist[x] <- dist(gpxDF[x-1,"lon"],gpxDF[x-1,"lat"],gpxDF[x,"lon"],gpxDF[x,"lat"])
    gpxDF$vm[x] <- gpxDF$dist[x]/as.numeric(gpxDF[x,"time"] - gpxDF[x-1,"time"])
  }
 }
# Calculo de distancias acumuladas
gpxDF$disAcu <- cumsum(gpxDF$dist)

## Visualización de mapas y gráficos
centroide <- cbind(mean(gpxDF$lon),mean(gpxDF$lat))

map.base <- get_googlemap(
  center= centroide,
  maptype = 'hybrid', ## (roadmap, terrain, satellite, hybrid)
  zoom = 12, ## 
  scale = 2, ## 
)
# mapa google maps con recorrido
mapa <- ggmap(map.base)+geom_path(data = gpxDF, aes(x = gpxDF$lon, y = gpxDF$lat), colour = "blue", alpha = 0.5, size = 2)

png("RmapaAnillo.png", width=800, height=800, res=120)
print(mapa)
dev.off()
# mapa openstreetmap con recorrido
osm.map <- ggmap(get_map(location = centroide, zoom=12, source='osm'))+geom_path(data = gpxDF, aes(x = gpxDF$lon, y = gpxDF$lat), colour = "blue", alpha = 0.5, size = 2)

png("osmap.png", width=800, height=800, res=120)
print(osm.map)
dev.off()
# Perfiles
perfil.tiempo <- ggplot(gpxDF, aes(x=time, y=ele))+ geom_line(colour = "red", size = 1)+ylab("Altura") + xlab("Hora")+ ggtitle("Perfil longitudinal - hora")+theme(axis.text.x = element_text(angle = 90))
perfil.velocidad <- ggplot(gpxDF, aes(x=disAcu, y=vm))+ geom_line(colour = "orange", size = 1)+ylab("Velocidad m/s") + xlab("Distancia")+ ggtitle("Velocidad media")+theme(axis.text.x = element_text(angle = 90))
perfil.distancia <- ggplot(gpxDF, aes(x=disAcu, y=ele))+ geom_line(colour = "blue", size = 1)+ylab("Altura") + xlab("Distancia")+ ggtitle("Perfil longitudinal - distancia")+theme(axis.text.x = element_text(angle = 90))

png("RperfilAnillo.png", width=800, height=800, res=120)
grid.arrange(perfil.tiempo, perfil.velocidad, perfil.distancia, ncol=1, main = 'Anillo ciclista de Madrid')
dev.off()

{% endhighlight %}

### Salidas de R

> Mapa google maps con el recorrido:

<figure>
	<img src="/images/RmapaAnillo.png">
</figure>

> Mapa OpenStreetMap con el recorrido:

<figure>
	<img src="/images/osmap.png">
</figure>

> Perfiles:

<figure>
	<img src="/images/RperfilAnillo.png">
</figure>


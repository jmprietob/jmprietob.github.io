---
layout: post
title:  "Extraer datos de netcdf"
date:   2014-02-17 22:00:00
categories: programacion
comments: true
---

Tras una consulta de a que hora iba a nevar en un sitio, se me ocurrió extraer para unas coordenadas dadas los datos de un netcdf de un modelo numérico de predicción y crear una gráfica con los datos. Este es el resultado:
<figure>
	<img src="/images/Rplot.png">
</figure>
El proyecto lo he realizado en [R](http://www.r-project.org/) aprovechando parte del código de un nota [anterior](http://jmprietob.github.io/prediccion/prediccion-07-02-2014/). La siguiente fase sería crear una pequeña aplicación web (estoy investigando [shiny](http://www.rstudio.com/shiny/)) donde el usuario introduce unas coordenadas y se generan las gráficas para ese punto.

### Código R

{% highlight ruby %}
#Librerias utilizadas
library(raster)
library(rgdal)
library(ggplot2)
library(gridExtra)

archivo <- "datosnetcdf.nc"
idx <- seq(as.POSIXct('2014-02-17 01:00:00', tz="UTC"),
	as.POSIXct('2014-02-21 00:00:00', tz="UTC"), 'hour')
idx <- as.POSIXct(idx)

## Leemos las diferentes variables
t <- stack(archivo, varname = "temp")
t <- t-273# pasar a grados C
proj4string(t) <- CRS("+proj=lcc +lon_0=-14.1 +lat_0=34.823 +lat_1=43 +lat_2=43 +x_0=536402.3 +y_0=-18558.61 +units=km +ellps=WGS84")
s <- stack(archivo, varname = "snowlevel")
proj4string(s) <- CRS("+proj=lcc +lon_0=-14.1 +lat_0=34.823 +lat_1=43 +lat_2=43 +x_0=536402.3 +y_0=-18558.61 +units=km +ellps=WGS84")
p <- stack(archivo, varname = "prec")
proj4string(p) <- CRS("+proj=lcc +lon_0=-14.1 +lat_0=34.823 +lat_1=43 +lat_2=43 +x_0=536402.3 +y_0=-18558.61 +units=km +ellps=WGS84")
snow <- stack(archivo, varname = "snow_prec")
proj4string(snow) <- CRS("+proj=lcc +lon_0=-14.1 +lat_0=34.823 +lat_1=43 +lat_2=43 +x_0=536402.3 +y_0=-18558.61 +units=km +ellps=WGS84")

pto <-cbind(-0.285628,42.7244749) #Panticosa 
punto = SpatialPoints(pto,CRS("+proj=longlat +datum=WGS84 +ellps=WGS84 +towgs84=0,0,0"))
projLCC <- projection(t)
#Transformo el punto a las coordenadas del netcdf
puntoLCC <- spTransform(punto, CRS(projLCC)) 

#Extraigo datos a un data frame
vals <- extract(t, puntoLCC,ncol = 2)
datos <- data.frame(t(vals))
vsl <- extract(s, puntoLCC,ncol = 2)
datos$snow_level <- t(vsl)
vpreci <- extract(p, puntoLCC,ncol = 2)
datos$lluvia <- t(vpreci)
spreci <- extract(snow, puntoLCC,ncol = 2)
datos$nieve <- t(spreci)
datos$time<-idx
   
#Creo las imágenes
t_plot <- ggplot(datos, aes(x=time, y=t.vals.))+ geom_line(colour = "red", size = 1)+
	ylab("Temperatura ºC)") + xlab("Fecha")+ ggtitle("Temperatura")+
	scale_x_datetime(breaks = date_breaks("4 hours"),labels = date_format("%d-%m %H h"))+
	theme(axis.text.x = element_text(angle = 90))
s_plot <- ggplot(datos, aes(x=time, y=nieve))+geom_bar(stat="identity",fill="orange", colour="orange")+
	ylab("Precipitación (mm/h)") + xlab("Fecha") + ggtitle("Precipitación - nieve")+
	scale_x_datetime(breaks = date_breaks("4 hours"),labels = date_format("%d-%m %H h"))+
	theme(axis.text.x = element_text(angle = 90))
sl_plot <- ggplot(datos, aes(x=time, y=snow_level)) + geom_line(colour = "green", size = 1)+
	ylab("Altura (m)") + xlab("Fecha") + ggtitle("Snow level")+
	scale_x_datetime(breaks = date_breaks("4 hours"),labels = date_format("%d-%m %H h"))+
	theme(axis.text.x = element_text(angle = 90))
p_plot <- ggplot(datos, aes(x=time, y=lluvia))+geom_bar(stat="identity",fill="blue", colour="blue")+
	ylab("Precipitación (mm/h)") + xlab("Fecha") + ggtitle("Precipitación - lluvia")+
	scale_x_datetime(breaks = date_breaks("4 hours"),labels = date_format("%d-%m %H h"))+
	theme(axis.text.x = element_text(angle = 90))
titulo <- paste("Datos para ",pto[,1],pto[,2])
#El multiplot
multiplot<-grid.arrange(t_plot, sl_plot, s_plot, p_plot, ncol=2, main = titulo)
{% endhighlight %}
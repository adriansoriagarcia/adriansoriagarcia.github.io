---
layout: single
title: Desplegar aplicación en servidor web.
excerpt: "En este artículo vamos a ver como desplegar una aplicación en un servidor web. En este caso el servidor web es 000webhost."
date: 2021-09-28
classes: wide
header:
  teaser: /assets/images/htb-writeup-ready/ready_logo.png
  teaser_home_page: true
  icon: /assets/images/logo-ionic.svg
categories:
  - programación
  - ionic
tags:
  - despliegue
  - plantilla
  - proyectos
  - angular

---


## Preparación

Para ello dentro de la carpeta de ionic escribimos el comando ionic server.

![](/assets/images/ionic-desplegar-aplicacion/1.PNG)

Seguidamente con el comando ionic build compilamos el proyecto.

![](/assets/images/ionic-desplegar-aplicacion/2.PNG)

En 000webhost creamos una nueva web.

![](/assets/images/ionic-desplegar-aplicacion/3.PNG)

Subimos los archivos correspondientes de nuestro proyecto.

![](/assets/images/ionic-desplegar-aplicacion/4.PNG)

## Ejecución

Y comprobamos que se puede ver correctamente.

![](/assets/images/ionic-desplegar-aplicacion/5.PNG)

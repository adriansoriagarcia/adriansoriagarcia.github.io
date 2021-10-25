---
layout: single
title: Detectar conexión con dispositivo Android "real".
excerpt: "En este artículo vamos a configurar los archivos correspondientes para que podamos conectar nuestro dispositivo para hacer pruebas reales."
date: 2021-10-01
classes: wide
header:
  teaser: /assets/images/htb-writeup-ready/ready_logo.png
  teaser_home_page: true
  icon: /assets/images/logo-ionic.svg
categories:
  - programación
  - ionic
  - android
tags:
  - despliegue
  - plantilla
  - proyectos
  - angular
  - local
  - emulador

---


## Preparación

Abrimos el Android Studio y seguidamente la ventana de android SDK para poder ver la ruta que necesitaremos a continuación.

![](/assets/images/ionic-android-real/1.PNG)

Con la ruta que hemos copiado antes la buscamos en el explorador de archivos.

![](/assets/images/ionic-android-real/2.PNG)

Entramos en la carpeta platform-tools

![](/assets/images/ionic-android-real/3.PNG)

Desde el buscador de windows, abrimos el editor de variables.

![](/assets/images/ionic-android-real/4.PNG)

Pulsamos en variables de entorno, y nos dirigimos a variables del sistema.

![](/assets/images/ionic-android-real/5.PNG)

Hacemos doble click sobre la variable que deseamos modificar en este caso PATH, y la modificamos por la ruta copiada anteriormente.

![](/assets/images/ionic-android-real/6.PNG)


## Ejecución

Una vez que hayas configurado PATH para acceder al SDK podrá ejecutar el comando adb que entre otras muchas cosas permite detectar los dispositivos y emuladores conectados al ordenador:
Conecta por cable tu dispositivo Android al ordenador, usando un cable USB que permita la transferencia de datos (no sólo para carga).
Activa el "Modo desarrollador" en tu dispositivo (depende del modelo y versión de Android) y activa el "Modo de depuración USB" (también depende del modelo y versión de Android).
Ejecuta el comando "adb devices".
Si todo ha ido bien, te deberá aparecer una lista de dispositivos detectados que no debe estar vacía. Debe aparecer un identificador con dígitos hexadecimales.

![](/assets/images/ionic-android-real/7.PNG)

Una vez que se detecte el dispositivo "real" ejecuta el proyecto de Android Studio en él (debería aparecer en la barra de herramientas el nombre del dispositivo, junto al botón de ejecución)


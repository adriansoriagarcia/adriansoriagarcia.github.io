---
layout: single
title: Aplicación adivinar número.
excerpt: "En este artículo vamos a desarrolla la aplicación de Adivinar un número siguiendo los pasos y realizando las siguiente mejoras: Posibilitar el reinicio de la partida. Una vez que se detecte que se ha adivinado el número secreto, se debe volver a activar y desactivar los botones correspondientes para poder iniciar una nueva partida.
Contar el número de veces que se acierta, mostrando ese contador en pantalla.
Limitar el número de intentos de cada partida al valor desees. Una vez que el usuario alcance ese límite se mostrará un mensaje en pantalla y se reiniciará una nueva partida.
Contar el número de veces que se falla por completar el límite de intentos, mostrando ese contador en pantalla. "

date: 2021-09-28
classes: wide
header:
  teaser: /assets/images/htb-writeup-ready/ready_logo.png
  teaser_home_page: true
  icon: /assets/images/programacion.png
categories:
  - programación
  - thunkable
tags:
  - herramientas
  - desarrollo
  - movil
  - interfaz

---


## Pasos

Este es el resultado final de nuestra aplicación.

```
El botón reiniciar pone a 0 el número de aciertos y el intentos por fallos ya que si no siguen sumando hasta que el usuario desea reiniciarlo.
Cada vez que terminamos la partida y pulsamos comenzar el numero de intentos se pone a 0 y el intentos disponibles vuelve a 5.
```

![](/assets/images/thunkable-adivina-numero/1.PNG)

Este es el resultado final de nuestra aplicación.

![](/assets/images/thunkable-adivina-numero/2.PNG)


![](/assets/images/thunkable-adivina-numero/3.PNG)

![](/assets/images/thunkable-adivina-numero/4.PNG)


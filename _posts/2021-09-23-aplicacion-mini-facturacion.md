---
layout: single
title: Aplicación mini-facturación.
excerpt: "En este artículo vamos a desarrollar una aplicación que ofrezca al usuario un teclado numérico en pantalla que debes diseñar con botones, añadiendo un botón por cada dígito numérico del 0 al 9 y otro para el punto que pueda usarse para cantidades con decimales.

A medida que el usuario vaya pulsando los botones, deberá aparecer en una etiqueta (Label) la cantidad numérica que se debe ir construyendo por cada pulsación de los botones, añadiendo cada nuevo dígito en la parte derecha. Es decir, si el usuario pulsa el botón 5, en la etiqueta aparecerá el valor 5, y si a continuación el usuario pulsa el botón 3, en la etiqueta aparecerá el valor 53, y así sucesivamente.

Añade también un botón para que el usuario pueda borrar completamente el valor que se haya introducido anteriormente.

Ese valor introducido por el usuario corresponderá al valor de un artículo de una supuesta compra. Un nuevo botón se debe encargar de sumar el IVA (21%) al importe que haya introducido el usuario. El valor resultante se mostrará en otro Label.

Procura que el diseño de la pantalla sea agradable al usuario, añadiendo otros Label que permitan identificar el importe introducido y el resto de la información que se muestre.

En un nuevo Label, y con 2 botones más, el usuario podrá indicar la cantidad de productos que ha comprado con ese importe. Por defecto aparecerá el valor 1 para esa cantidad, pero con uno de los botones el usuario puede ir incrementado ese valor, y con el otro botón podrá decrementarlo. El importe calculado anteriormente deberá tener en cuenta ahora la cantidad de productos que compra el usuario, por lo que deberás multiplicar la cantidad por el importe unitario.

Por último, puedes añadir a la aplicación la posibilidad de aplicar diversos descuentos (con porcentajes) con botones que hagan distintos descuentos, por ejemplo, por ser socio, por cliente VIP y similares."
date: 2021-09-23
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

Imagen del diseño.

![](/assets/images/aplicacion-mini-facturación/1.PNG)

A continuación vamos a mostrar el código.

![](/assets/images/aplicacion-mini-facturación/2.PNG)


![](/assets/images/aplicacion-mini-facturación/3.PNG)


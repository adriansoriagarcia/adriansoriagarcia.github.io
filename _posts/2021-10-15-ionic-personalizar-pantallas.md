---
layout: single
title: Personalizar estilos según pantalla.
excerpt: "En este artículo vamos a modificar el archivo css de nuestro proyecto para crear al menos 3 estilos diferentes de tamaño de pantalla, (pequeña, mediana con rangos min-width max-width y para pantalla grande) para los elementos que desees de la aplicación.."
date: 2021-10-15
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
  - local

---

A continuación muestro el código css y el código del botón añadido.

## Código HTML
```
 <ion-button class="boton" expand="block">Adivina</ion-button>

```

## Código CSS 
```
.ion-padding {
  background-color:lightblue;
}

@media only screen and (min-width: 1000px) {
  .ion-padding {
    background-color:blue;
  }
}

@media screen and (max-width: 600px) {
  .ion-padding {
    background-color:lightgreen;
  }
}


.boton {
  color: black;
  width: 10%;
  margin-left: auto;
  margin-right: auto;
}

@media only screen and (max-width: 600px) {
  .boton {
    display: none;
  }
}

```



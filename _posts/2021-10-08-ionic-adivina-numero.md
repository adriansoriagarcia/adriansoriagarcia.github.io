---
layout: single
title: Aplicación de toma de contacto: Adivina número.
excerpt: "En este artículo vamos a desarrollar la aplicación Adivina número, cuya elaboración se a continuación. Esta aplicación servirá para ir tomando contacto con Ionic y los elementos que componen una aplicación de este tipo."
date: 2021-10-08
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


## Preparación

Creamos un nuevo proyecto con el siguiente comando.

![](/assets/images/ionic-adivina-numero/1.PNG)

Lo ejecutamos con el siguiente comando.

![](/assets/images/ionic-adivina-numero/2.PNG)

Abrimos el proyecto en visual studio code.

![](/assets/images/ionic-adivina-numero/3.PNG)

A continuación vemos todo el código HTML y TS.

![](/assets/images/ionic-adivina-numero/4.PNG)
![](/assets/images/ionic-adivina-numero/5.PNG)

## Código HTML
```
<ion-header [translucent]="true">
  <ion-toolbar>
    <ion-title>
      Adivina el número secreto
    </ion-title>
  </ion-toolbar>
</ion-header>

<ion-content [fullscreen]="true">
  <div class="ion-padding">
    <ion-input type="number" min="1" max="100" [(ngModel)]="num" placeholder="Introduce un número del 1 al 100">
    </ion-input>
    <p>El número secreto es {{ mayorMenor }} que el número introducido</p>
    <ion-button expand="block" (click)="compruebaNumero()">Adivina</ion-button>
  </div>
</ion-content>
```

## Código TS
```
import { Component } from '@angular/core';

@Component({
  selector: 'app-home',
  templateUrl: 'home.page.html',
  styleUrls: ['home.page.scss'],
})
export class HomePage {

  num: Number;
  NumSecret: Number = this.numAleatorio(1, 100);
  mayorMenor: string = "...";

  constructor() {
    console.log("El número secreto es: " + this.NumSecret);
  }

  numAleatorio(a, b) {
    return Math.round(Math.random()*(b-a)+parseInt(a));
  }

  compruebaNumero() {
    if (this.num)
      if (this.NumSecret < this.num){
        this.mayorMenor = "menor";
      }else if (this.NumSecret > this.num) {
        this.mayorMenor = "mayor";
      } else{
        this.mayorMenor = "igual";
      }
  }

}

```
## Ejecución

Ahora vemos las distintas comprobaciones.

![](/assets/images/ionic-adivina-numero/6.PNG)
![](/assets/images/ionic-adivina-numero/7.PNG)
![](/assets/images/ionic-adivina-numero/8.PNG)



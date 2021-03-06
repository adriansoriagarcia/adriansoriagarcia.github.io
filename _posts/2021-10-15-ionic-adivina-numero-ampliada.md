---
layout: single
title: Aplicación Adivina número - Ampliación con *ngIf.
excerpt: "En este artículo vamos a amplíar la funcionalidad de la aplicación Adivina número con las instrucciones siguientes. Así conocerás el funcionamiento de la directiva *ngIf de Angular para mostrar elementos condicionalmente."
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
    <ion-card *ngIf="mayorMenor=='igual'">
      <ion-card-header>
        ¡¡¡Enhorabuena!!!
      </ion-card-header>
      <ion-car-content>
        Has acertado, el número secreto es {{num}}
      </ion-car-content>
    </ion-card>
    <ion-button expand="block" *ngIf="mayorMenor!='igual'" (click)="compruebaNumero()">Adivina</ion-button>
    <ion-button expand="block" *ngIf="mayorMenor=='igual'" (click)="reinicia()">Volver a jugar</ion-button>
 </div>
</ion-content>

```

## Código TS (ampliación)
```
reinicia() {
    //reiniciamos las variables
    this.num = null;
    this.mayorMenor = "...";
    this.NumSecret = this.numAleatorio(1, 100);
    console.log("El número secreto es: " + this.NumSecret);
  }

```



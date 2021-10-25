---
layout: single
title: prueba de funcionamiento.
excerpt: "En este artículo vamos a desarrollar una aplicación donde vamos a utilizar un bucle for de angular en el HTML para poder mostrar una lista de usuarios."
date: 2021-10-22
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
  - bucles

---

Aquí adjunto el enlace de mi github para poder descargar el proyecto = https://github.com/adriansoriagarcia/responsive.git


A continuación muestro el código css y el código del botón añadido.

## Código HTML
```
<ion-header [translucent]="true">
  <ion-toolbar>
    <ion-title>
      Listado de personas
    </ion-title>
  </ion-toolbar>
</ion-header>

<ion-content [fullscreen]="true">
  <ion-grid class="contenido">
    <ion-row>
      <ion-col *ngFor="let user of users | async" size-xl="2" size-lg="2" size-md="4" size-sm="6" size-xs="12">
        <ion-card class="tarjetas">
          <ioncard-content>
            <ion-avatar>
              <img class="imagen" [src]="user.picture.medium">
            </ion-avatar>
            <h1> {{ user.name.first }} {{ user.name.last }}  </h1>
            <h3> {{ user.location.city }} {{user.location.state}} </h3>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam vitae porta lorem. Proin quis mattis nisi, a sollicitudin nisi. Sed arcu lectus, consequat non sagittis eleifend, fermentum maximus sapien.
          </ioncard-content>
        </ion-card>
      </ion-col>
    </ion-row>
  </ion-grid>
</ion-content>


```

## Código CSS 
```
#container {
  text-align: center;

  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;

  color: #8c8c8c;

  margin: 0;
}

#container a {
  text-decoration: none;
}

.contenido {
  background-color: aqua;
}

.tarjetas {
  background-color: lightgreen;
  color: black;
}
```

## Código PAGE.TS
```
import { Component } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { map } from 'rxjs/operators';

@Component({
  selector: 'app-home',
  templateUrl: 'home.page.html',
  styleUrls: ['home.page.scss'],
})
export class HomePage {

  users: any;

  constructor(private httpClient: HttpClient) {
    this.users = this.httpClient.get('https://randomuser.me/api/?results=20').pipe(map(res => res['results']))
   // console.log(this.users);
  }

}
```

## Código MODULE.TS
```
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { IonicModule } from '@ionic/angular';
import { FormsModule } from '@angular/forms';
import { HomePage } from './home.page';

import { HomePageRoutingModule } from './home-routing.module';
import { HttpClientModule } from '@angular/common/http';

@NgModule({
  imports: [
    CommonModule,
    FormsModule,
    IonicModule,
    HttpClientModule,
    HomePageRoutingModule
  ],
  declarations: [HomePage]
})
export class HomePageModule {}
```

## Resultado

A continuación vamos a ver como seria el resultado en diferentes pantallas.
![](/assets/images/ionic-responsive/1.PNG)
![](/assets/images/ionic-responsive/2.PNG)
![](/assets/images/ionic-responsive/3.PNG)

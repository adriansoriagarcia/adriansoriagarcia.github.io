---
layout: single
title: Aplicación "responsive" personalizada.
excerpt: "En este artículo vamos a desarrollar una aplicación parecida a la anterior, pero en este caso vamos a utilizar un documento JSON generado por nosotros para obtener la información.."
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

Aquí adjunto el enlace de mi github para poder descargar el proyecto = https://github.com/adriansoriagarcia/responsive-personalizado.git


A continuación muestro el código HTML, CSS y .

## Código HTML
```
<ion-header [translucent]="true">
  <ion-toolbar>
    <ion-title>
      Listado de personas personalizado
    </ion-title>
  </ion-toolbar>
</ion-header>

<ion-content [fullscreen]="true">
  <ion-input [(ngModel)]="filtro" placeholder="Introduce los datos del filtro">

  </ion-input>
  <ion-grid class="contenido">
    <ion-row>
      <ng-container *ngFor="let user of users | async">
      <ion-col  *ngIf="filtro == '' || user.Genero.includes(filtro) || user.Nombre.startsWith (filtro)"  size-xl="2" size-lg="2" size-md="4" size-sm="6" size-xs="12">
        <ion-card class="tarjetas">
          <ioncard-content>
            <h1> {{ user.Dni }} {{ user.Nombre }}  </h1>
            <h3> {{ user.Apellido }} {{user.Email}} </h3>
            <p class="desaparece"> {{ user.Ciudad }} {{user.Telefono}} </p>
            <p class="oculto"> {{ user.Cuenta }}  </p>
          </ioncard-content>
        </ion-card>
      </ion-col>
    </ng-container>
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
  background-color: plum;
}

.tarjetas {
  background-color: greenyellow;
  color: black;
}

@media only screen and (max-width: 600px) {
  .desaparece {
    display: none;
  }
}

@media only screen and (max-width: 400px) {
  .oculto {
    display: none;
  }
}

@media only screen and (max-width: 600px) {
  .tarjetas {
    color: white;
  }
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
  filtro: string = '';

  constructor(private httpClient: HttpClient) {
    this.users = this.httpClient.get('https://raw.githubusercontent.com/adriansoriagarcia/responsive-personalizado/master/datosaleatorios.json');
    console.log(this.users);
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

## Pasos a seguir
Cada registro de información se deberá mostrar en un elemento ion-card, debiendo adaptarse el número de columnas a diversos tamaños de pantalla. Los datos deberán ser obtenidos a partir de un documento JSON accesible desde una URL. 
Puedes generar el documento JSON usando un generador aleatorio como https://mockaroo.com/, o bien puedes editar a mano un documento a tu gusto.

```
Enlace al archivo JSON generado: https://github.com/adriansoriagarcia/responsive-personalizado/blob/master/datosaleatorios.json
```

Como los datos del archivo JSON viene directamente como un array (sin que tenga un array dentro de otra etiqueta como en el ejemplo anterior), no hace falta usar el método pipe y map, sólo así:
this.variable = this.httpClient.get('https://miserver/folder/arrayjson');

```
Linea utilizada en el archivo home.ts: this.users = this.httpClient.get('https://raw.githubusercontent.com/adriansoriagarcia/responsive-personalizado/master/datosaleatorios.json');
```

A los datos del documento JSON se debe acceder desde la web, de manera semejante a la aplicación de lista de contactos anterior. Por tanto, para que tu archivo JSON sea accesible desde la web debes alojarlo en algún servidor web. Una manera sencilla de hacerlo es subirlo a GitHub junto con tu proyecto.    

![](/assets/images/ionic-responsive-personalizada/json.PNG)

La aplicación deberá usar estilos CSS, aplicando estilos fijos a determinados elementos, y estilos variables a otros elementos en función del tipo de pantalla donde se visualice la aplicación.
```
.contenido {
  background-color: plum;
}

.tarjetas {
  background-color: greenyellow;
  color: black;
}

@media only screen and (max-width: 600px) {
  .desaparece {
    display: none;
  }
}

@media only screen and (max-width: 400px) {
  .oculto {
    display: none;
  }
}

@media only screen and (max-width: 600px) {
  .tarjetas {
    color: white;
  }
}
```

Se deberán usar elementos que se hagan visibles o invisibles en función del tipo de pantalla usando el atributo orientation de CSS.
```
@media only screen and (max-width: 600px) {
  .desaparece {
    display: none;
  }
}

@media only screen and (max-width: 400px) {
  .oculto {
    display: none;
  }
}
```

Debes hacer uso en la aplicación de Data Binding de Angular, haciendo un filtro de búsqueda como muestro en el videotutorial que hay enlazado tras esta tarea.
```
Lineas utilizadas en el archivo HTML
<ion-input [(ngModel)]="filtro" placeholder="Introduce los datos del filtro">
<ion-col  *ngIf="filtro == '' || user.Genero.includes(filtro) || user.Nombre.startsWith (filtro)">
```

Cambia el icono de la aplicación y la pantalla de presentación (splash screen) según las indicaciones que se dan en el apartado siguiente. 
## Como cambiar el icono de la aplicación y la pantalla de presentacion.
Crea una carpeta resources (si no existe) dentro del proyecto.
Crea o busca una imagen para el icono. Deberá ser una imagen PNG de tamaño 1024x1024 píxeles. Asígnale el nombre icon.png y almacénala en la carpeta resources.
Crea o busca una imagen para la pantalla de presentación (splash screen). Deberá ser una imagen PNG de tamaño 2732x2732 píxeles. Asígnale el nombre splash.png y almacénala en la carpeta resources.
Para aplicaciones a partir de Android 8.0 (nivel de API 26):
Haz una copia de esa misma imagen del icono pero de un tamaño 432x432 píxeles. Asígnale el nombre icon-foreground.png y almacénala en la carpeta resources/android (deberás crear esa carpeta android si no existe).
Crea o busca una imagen con las transparencias que desees para usar el icono de manera adaptativa (Designing Adaptive Icons). Debe ser una imagen de 432x432 píxeles. Asígnale el nombre icon-background.png y almacénala en la carpeta resources/android.
Deberá quedar una estructura similar a la siguiente:
![](/assets/images/ionic-responsive-personalizada/estructura.PNG)

## Instala cordova-res
Instala la aplicación cordova-res, si no lo has instalado previamente en el sistema:
```
npm install -g cordova-res
```
Genera las imágenes con los tamaños adecuados
Ejecuta (en la carpeta del proyecto) los comandos siguientes que necesites según el tipo de plataforma (ios o android) que desees:
```
cordova-res ios --skip-config --copy
cordova-res android --skip-config --copy
```
De esa manera se habrán generado automáticamente los archivos correspondientes al icono y a la splash para multitud de posibles tamaños de pantalla.

![](/assets/images/ionic-responsive-personalizada/estructura1.png)

##Compila e instala la aplicación en un dispositivo

Con el siguiente comando añadimos la plataforma Android al proyecto: "ionic capacitor add android".
Con este comando compilamos y ejecutamos directamente el proyecto en nuestro dispositivo: "ionic capacitor run android".


## Resultado

Desde el navegador web con ancho de pantalla completo.
![](/assets/images/ionic-responsive-personalizada/1.PNG)

Desde el navegador web con ancho de pantalla pequeño.
![](/assets/images/ionic-responsive-personalizada/2.PNG)

Desde un teléfono Android con orientación vertical.
![](/assets/images/ionic-responsive-personalizada/vistamovil1.jpg)

Desde un teléfono Android con orientación horizontal.
![](/assets/images/ionic-responsive-personalizada/vistamovil2.jpg)



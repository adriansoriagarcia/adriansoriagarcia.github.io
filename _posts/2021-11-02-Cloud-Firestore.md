---
layout: single
title: Como utilizar Cloud Firestore con Ionic.
excerpt: "En este artículo vamos a ver un ejemplo de como utilizar Cloud Firestore con Ionic"
date: 2021-11-02
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

Aquí adjunto el enlace de mi github para poder descargar el proyecto = https://github.com/adriansoriagarcia/ejemplo-firestore


## Pasos para configurar la base de datos en Google Cloud Firestore

Accedemos a la página web ` console.firebase.google.com` y creamos un proyecto.
![](/assets/images/ionic-ejemplo-cloud-firestore/1.PNG)

En este paso escribimos el nombre del proyecto y pulsamos continuar.
![](/assets/images/ionic-ejemplo-cloud-firestore/2.PNG)

Aquí podemos dejar activado o desactivar Google Analytics, en mi caso lo dejo activado.
![](/assets/images/ionic-ejemplo-cloud-firestore/3.PNG)

En la siguiente ventana nos mostrala las condiciones y configuracion, pulsamos en crear proyecto.
![](/assets/images/ionic-ejemplo-cloud-firestore/4.PNG)
![](/assets/images/ionic-ejemplo-cloud-firestore/5.PNG)
![](/assets/images/ionic-ejemplo-cloud-firestore/6.PNG)

Cuando el proyecto ha terminado de crearse, nos dirigimos a Firestore Database en el menú izquierdo y creamos una base de datos.
![](/assets/images/ionic-ejemplo-cloud-firestore/7.PNG)

En este paso seleccionamos comenzar en modo de prueba y pulsamos en siguiente.
![](/assets/images/ionic-ejemplo-cloud-firestore/8.PNG)

Seleccionamos la región, para ello seleccionamos la europea.
![](/assets/images/ionic-ejemplo-cloud-firestore/9.PNG)

En la pestaña siguiente podemos comprobar los datos que contiene nuestra base de datos.
![](/assets/images/ionic-ejemplo-cloud-firestore/10.PNG)

## Creamos un nuevo proyecto en Ionic
```
ionic start ejemplo-firestore blank
cd ejemplo-firestore 
npm install firebase @angular/fire
```

## Configuración de la base de datos

En el menú izquierdo, pulsamos en Project Overview y pulsamos en el botón señalado en la imagen.
![](/assets/images/ionic-ejemplo-cloud-firestore/11.PNG)

Indicamos un nombre para registrar la app.
![](/assets/images/ionic-ejemplo-cloud-firestore/12.PNG)

En este paso nos mostrara los datos de configuración.
![](/assets/images/ionic-ejemplo-cloud-firestore/13.PNG)



## Archivo environments/environment.ts
```
export const environment = {
  production: false,
  firebaseConfig: {
    apiKey: "????????????",
    authDomain: "????????????",
    databaseURL: "????????????",
    projectId: "????????????",
    storageBucket: "????????????",
    messagingSenderId: "????????????",
    appId: "????????????"
  }
};
```

## Archivo app.module.ts
```
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { RouteReuseStrategy } from '@angular/router';

import { IonicModule, IonicRouteStrategy } from '@ionic/angular';

import { AppComponent } from './app.component';
import { AppRoutingModule } from './app-routing.module';

import { AngularFireModule } from '@angular/fire/compat';
import { AngularFirestoreModule } from '@angular/fire/compat/firestore';
import { environment } from '../environments/environment';

@NgModule({
  declarations: [AppComponent],
  entryComponents: [],
  imports: [BrowserModule, IonicModule.forRoot(), AppRoutingModule,
    AngularFireModule.initializeApp(environment.firebaseConfig),
    AngularFirestoreModule],
  providers: [{ provide: RouteReuseStrategy, useClass: IonicRouteStrategy }],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

## Como declarar el tipo de objeto que vamos a almacenar en la base de datos.

Ejecutamos el siguiente comando desde el CMD para generar un archivo donde declaramos la estructura de datos que se va a utilizar.
`ionic generate interface Tarea`

Con el comando anterior hemos creado un archivo dentro de la carpeta src/app.
![](/assets/images/ionic-ejemplo-cloud-firestore/14.PNG)
![](/assets/images/ionic-ejemplo-cloud-firestore/15.PNG)

## Arichivo tarea.ts
```
export interface Tarea {
    titulo: string;
    descripcion: string;
}
```

## Clase para métodos de acceso a la base de datos

Con el siguiente comando creamos una nueva clase dentro del proyecto.
`ionic generate service firestore`

 Estos son los archivos que se generan.
![](/assets/images/ionic-ejemplo-cloud-firestore/17.PNG)
![](/assets/images/ionic-ejemplo-cloud-firestore/18.PNG)

## Insertar registros

## Archivo firestore.service.ts
```
import { Injectable } from '@angular/core';

import { AngularFirestore } from '@angular/fire/compat/firestore';

@Injectable({
  providedIn: 'root'
})
export class FirestoreService {

  constructor(private angularFirestore: AngularFirestore) { 
  }

  public insertar(coleccion, datos) {
    return this.angularFirestore.collection(coleccion).add(datos);
  } 
 
}
```

## Archivo home.page.ts
Va a crear una propiedad de la clase donde se va a almacenar un objeto del tipo de dato que se ha declarado en el interface.
`tareaEditando: Tarea;`
Esa propiedad se crea como un objeto vacío en el constructor de la clase:
`this.tareaEditando = {} as Tarea;`
Como parámetro del método constructor se debe inyectar el servicio (firestore) que hemos creado anteriormente para que puedan ser usados en esta clase los métodos (insertar, consultar, etc) que se vayan creando en el archivo firestore.service.ts.
`constructor(private firestoreService: FirestoreService)`
También se va a preparar un método llamado clicBotonInsertar que se ejecutará cuando el usuario pulse un botón, que se encargará de tomar los datos que haya en la propiedad tareaEditando para insertarlos en la base de datos, llamando al método insertar que hemos hecho antes en el servicio que hemos llamado FirestoreService.
`this.firestoreService.insertar("tareas", this.tareaEditando)`
Observa que tras la llamada a insertar se usa el método then() de JavaScript para ejecutar un bloque de código una vez que finalice el código anterior (en este caso la inserción en la base de datos) que se hace de manera asíncrona. Aquí se va a mostrar un mensaje por consola para señalar que se ha realizado la acción correctamente y se vuelve a dejar vacío el objeto tareaEditando, de manera que quede preparado para seguir añadiendo nuevos datos.

`console.log('Tarea creada correctamente!');
this.tareaEditando= {} as Tarea;`

También se hace uso de la opción error del método then() para ejecutar código si se ha producido algún error. En este ejemplo se va a mostrar el error en la consola.

`(error) => {
    console.error(error);
}`

El código completo es el siguiente.
```
import { Component } from '@angular/core';
import { FirestoreService } from '../firestore.service';
import { Tarea } from '../tarea';

@Component({
  selector: 'app-home',
  templateUrl: 'home.page.html',
  styleUrls: ['home.page.scss'],
})
export class HomePage {

  tareaEditando: Tarea;  

  constructor(private firestoreService: FirestoreService) {
    // Crear una tarea vacía
    this.tareaEditando = {} as Tarea;
  } 
  
  clicBotonInsertar() {
    this.firestoreService.insertar("tareas", this.tareaEditando).then(() => {
      console.log('Tarea creada correctamente!');
      this.tareaEditando= {} as Tarea;
    }, (error) => {
      console.error(error);
    });
  }

}
```

## Archivo home.page.html
Para que el usuario introduzca datos, creamos un formulario con etiquetas Ionic. Vamos a hacer databinding con el objeto TareaEditando.
`[(ngModel)]="tareaEditando.titulo"`
`[(ngModel)]="tareaEditando.descripcion"`

Además añadimos un botón para que ejecute el método clicBotonInsertar() que hemos implementado antes en el archivo de TypeScript.
`<ion-button (click)="clicBotonInsertar()">`

El código completo es el siguiente.
```
<ion-header>
  <ion-toolbar>
    <ion-title>
      Ejemplo Firestore
    </ion-title>
  </ion-toolbar>
</ion-header>

<ion-content [fullscreen]="true">
  <ion-item>
    <ion-label>Título</ion-label>
    <ion-input [(ngModel)]="tareaEditando.titulo"></ion-input>
  </ion-item>
  <ion-item>
    <ion-label>Descripción</ion-label>
    <ion-input [(ngModel)]="tareaEditando.descripcion"></ion-input>
  </ion-item>
  <ion-button (click)="clicBotonInsertar()">Añadir tarea</ion-button>
</ion-content>
```

Así queda visualmente nuestro proyecto.
![](/assets/images/ionic-ejemplo-cloud-firestore/23.PNG)

Insertamos datos para comprobar que se muestran en Firestore.
![](/assets/images/ionic-ejemplo-cloud-firestore/22.PNG)

## Obtener lista de registros

Igual que hemos realizado en el paso anterior de insertar registros, vamos a hacer lo mismo, crearemos un metodo en el archivo firestore.service.ts y modificamos el archivo HTML.
## Archivo firestore.service.ts
 ` public consultar(coleccion) {
    return this.angularFirestore.collection(coleccion).snapshotChanges();
  }`
  
## Archivo home.page.ts  
```
  arrayColeccionTareas: any = [{
    id: "",
    data: {} as Tarea
   }];

  constructor(private firestoreService: FirestoreService) {
    this.obtenerListaTareas();
  }

  obtenerListaTareas(){
    this.firestoreService.consultar("tareas").subscribe((resultadoConsultaTareas) => {
      this.arrayColeccionTareas = [];
      resultadoConsultaTareas.forEach((datosTarea: any) => {
        this.arrayColeccionTareas.push({
          id: datosTarea.payload.doc.id,
          data: datosTarea.payload.doc.data()
        });
      })
    });
  }
```

## Archivo home.page.html
```
<h1>LISTA DE TAREAS</h1>
  <ion-list>
    <ion-item *ngFor="let documentTarea of arrayColeccionTareas">
      <ion-grid>
        <ion-row>
          <h2>{{documentTarea.data.titulo}}</h2>
        </ion-row>
        <ion-row>
          <p>{{documentTarea.data.descripcion}}</p>
        </ion-row>
      </ion-grid>
    </ion-item>
  </ion-list>
```

## Borrado de datos

## Archivo firestore.service.ts
 Para borrar registros de Firestore se requiere conocer el ID del documento que se va a eliminar, para ello con el siguiente método, va a pedir como parámetro el ID del documento.
 ```
 public borrar(coleccion, documentId) {
   return this.angularFirestore.collection(coleccion).doc(documentId).delete();
 }
 ```
 
## Archivo home.page.ts
 
 Creamos la propiedad idTareaSelec para almacenar el Id que deseamos borrar. Pasamos el valos al método.borrar() anterior. El método selecTarea() se encarga de almacenar en idTareaSelec el ID.
 
 ```
 idTareaSelec: string;

  selecTarea(tareaSelec) {
    console.log("Tarea seleccionada: ");
    console.log(tareaSelec);
    this.idTareaSelec = tareaSelec.id;
    this.tareaEditando.titulo = tareaSelec.data.titulo;
    this.tareaEditando.descripcion = tareaSelec.data.descripcion;
  }

  clicBotonBorrar() {
    this.firestoreService.borrar("tareas", this.idTareaSelec).then(() => {
      // Actualizar la lista completa
      this.obtenerListaTareas();
      // Limpiar datos de pantalla
      this.tareaEditando = {} as Tarea;
    })
  }
 ```
 
## Archivo home.page.html
Añadir a cada item de la lista la llamada al método selecTarea cuando se seleccione un determinado item. Se recorre el array arrayColeccionTareas, cargando en la variable documentTarea cada uno de los elementos de ese array. Esa variable documentTarea es la que se pasa como parámetro en la llamada a selecTarea() que se ha implementado antes para almacenar el ID y los datos de la tarea seleccionada por el usuario.
```
  <ion-list>
    <ion-item *ngFor="let documentTarea of arrayColeccionTareas" (click)="selecTarea(documentTarea)">
      ...
    </ion-item>
  </ion-list>
```  
En este ejemplo se ha añadido un botón para eliminar el item que se encuentre seleccionado: 
`<ion-button (click)="clicBotonBorrar()">Borrar tarea</ion-button>`
---
layout: single
title: Proyecto final de Ionic con Firebase.
excerpt: "En este artículo vamos a ver el resultado final de una app multiplataforma de Cloud Firestore con Ionic"
date: 2021-12-21
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

Aquí adjunto el enlace de mi github para poder descargar el proyecto = 
* https://github.com/adriansoriagarcia/proyecto-manitas
Aquí adjunto el enlace de un video de demostracion de la app = 
* https://drive.google.com/file/d/1pmf9JPR-RkzRzR-MAMU_Ocsa7in7pbxR/view?usp=sharing

## Pasos para configurar la base de datos en Google Cloud Firestore

Accedemos a la página web ` console.firebase.google.com` y creamos un proyecto.
![](/assets/images/ionic-proyecto-final/1.PNG)

En este paso escribimos el nombre del proyecto y pulsamos continuar.
![](/assets/images/ionic-proyecto-final/2.PNG)

Aquí podemos dejar activado o desactivar Google Analytics, en mi caso lo dejo activado.
![](/assets/images/ionic-proyecto-final/3.PNG)

En la siguiente ventana nos mostrala las condiciones y configuracion, pulsamos en crear proyecto.
![](/assets/images/ionic-proyecto-final/4.PNG)
![](/assets/images/ionic-proyecto-final/5.PNG)
![](/assets/images/ionic-proyecto-final/6.PNG)

Cuando el proyecto ha terminado de crearse, nos dirigimos a Firestore Database en el menú izquierdo y creamos una base de datos.
![](/assets/images/ionic-proyecto-final/7.PNG)

En este paso seleccionamos comenzar en modo de prueba y pulsamos en siguiente.
![](/assets/images/ionic-proyecto-final/8.PNG)

Seleccionamos la región, para ello seleccionamos la europea.
![](/assets/images/ionic-proyecto-final/9.PNG)

En la pestaña siguiente podemos comprobar los datos que contiene nuestra base de datos.
![](/assets/images/ionic-proyecto-final/10.PNG)

## Creamos un nuevo proyecto en Ionic
```
ionic start ejemplo-firestore blank
cd ejemplo-firestore 
npm install firebase @angular/fire
```

## Configuración de la base de datos

En el menú izquierdo, pulsamos en Project Overview y pulsamos en el botón señalado en la imagen.
![](/assets/images/ionic-proyecto-final/11.PNG)

Indicamos un nombre para registrar la app.
![](/assets/images/ionic-proyecto-final/12.PNG)

En este paso nos mostrara los datos de configuración.
![](/assets/images/ionic-proyecto-final/13.PNG)



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
`ionic generate interface reparacion`

Con el comando anterior hemos creado un archivo dentro de la carpeta src/app.
![](/assets/images/ionic-proyecto-final/14.PNG)
![](/assets/images/ionic-proyecto-final/15.PNG)

## Arichivo reparacion.ts
```
export interface Reparacion {
    nombre: string;
    fecha: string;
    lugar: string;
    imagen: string;
    precio: string;
}
```

## Clase para métodos de acceso a la base de datos

Con el siguiente comando creamos una nueva clase dentro del proyecto.
`ionic generate service firestore`

 Estos son los archivos que se generan.
![](/assets/images/ionic-proyecto-final/17.PNG)
![](/assets/images/ionic-proyecto-final/18.PNG)

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
`reparacionEditando: Reparacion;`
Esa propiedad se crea como un objeto vacío en el constructor de la clase:
`this.reparacionEditando = {} as Reparacion;`
Como parámetro del método constructor se debe inyectar el servicio (firestore) que hemos creado anteriormente para que puedan ser usados en esta clase los métodos (insertar, consultar, etc) que se vayan creando en el archivo firestore.service.ts.
`constructor(private firestoreService: FirestoreService)`
También se va a preparar un método llamado clicBotonInsertar que se ejecutará cuando el usuario pulse un botón, que se encargará de tomar los datos que haya en la propiedad reparacionEditando para insertarlos en la base de datos, llamando al método insertar que hemos hecho antes en el servicio que hemos llamado FirestoreService.
`this.firestoreService.insertar("reparaciones", this.reparacionEditando)`
Observa que tras la llamada a insertar se usa el método then() de JavaScript para ejecutar un bloque de código una vez que finalice el código anterior (en este caso la inserción en la base de datos) que se hace de manera asíncrona. Aquí se va a mostrar un mensaje por consola para señalar que se ha realizado la acción correctamente y se vuelve a dejar vacío el objeto reparacionEditando, de manera que quede preparado para seguir añadiendo nuevos datos.

`console.log('Reparacion creada correctamente!');
this.reparacionEditando= {} as Reparacion;`

También se hace uso de la opción error del método then() para ejecutar código si se ha producido algún error. En este ejemplo se va a mostrar el error en la consola.

`(error) => {
    console.error(error);
}`

El código completo es el siguiente.
```
import { Component } from '@angular/core';
import { FirestoreService } from '../firestore.service';
import { Reparacion } from '../reparacion';

@Component({
  selector: 'app-home',
  templateUrl: 'home.page.html',
  styleUrls: ['home.page.scss'],
})
export class HomePage {

  reparacionEditando: Reparacion; 

  constructor(private firestoreService: FirestoreService) {
    // Crear una reparacion vacía
    this.reparacionEditando = {} as Reparacion;
  } 
  
  clicBotonInsertar() {
    this.firestoreService.insertar("reparaciones", this.reparacionEditando).then(() => {
      console.log('Reparacion creada correctamente!');
      this.reparacionEditando= {} as Reparacion;
    }, (error) => {
      console.error(error);
    });
  }

}
```

## Archivo home.page.html
Para que el usuario introduzca datos, creamos un formulario con etiquetas Ionic. Vamos a hacer databinding con el objeto reparacionEditando.
`[(ngModel)]="reparacionEditando.nombre"`
`[(ngModel)]="reparacionEditando.fecha"`
`[(ngModel)]="reparacionEditando.lugar"`
`[(ngModel)]="reparacionEditando.precio"`
`[(ngModel)]="reparacionEditando.imagen"`

Además añadimos un botón para que ejecute el método clicBotonInsertar() que hemos implementado antes en el archivo de TypeScript.
`<ion-button (click)="clicBotonInsertar()">`

El código completo es el siguiente.
```
<ion-header>
  <ion-toolbar>
    <ion-title>
      Proyecto manitas
    </ion-title>
  </ion-toolbar>
</ion-header>

<ion-content [fullscreen]="true">
  <ion-item>
    <ion-label>Nombre</ion-label>
    <ion-input [(ngModel)]="reparacionEditando.nombre"></ion-input>
  </ion-item>
  <ion-item>
    <ion-label>Fecha</ion-label>
    <ion-input [(ngModel)]="reparacionEditando.fecha"></ion-input>
  </ion-item>
  <ion-item>
    <ion-label>Lugar</ion-label>
    <ion-input [(ngModel)]="reparacionEditando.lugar"></ion-input>
  </ion-item>
  <ion-item>
    <ion-label>Precio</ion-label>
    <ion-input [(ngModel)]="reparacionEditando.precio"></ion-input>
  </ion-item>
  <ion-item>
    <ion-label>Imagen</ion-label>
    <ion-input [(ngModel)]="reparacionEditando.imagen"></ion-input>
  </ion-item>
  <ion-button (click)="clicBotonInsertar()">Añadir reparacion</ion-button>
</ion-content>
```

Así queda visualmente nuestro proyecto.
![](/assets/images/ionic-proyecto-final/23.PNG)

Insertamos datos para comprobar que se muestran en Firestore.
![](/assets/images/ionic-proyecto-final/22.PNG)

## Obtener lista de registros

Igual que hemos realizado en el paso anterior de insertar registros, vamos a hacer lo mismo, crearemos un metodo en el archivo firestore.service.ts y modificamos el archivo HTML.
## Archivo firestore.service.ts
```
public consultar(coleccion) {
  return this.angularFirestore.collection(coleccion).snapshotChanges();
}
```
  
## Archivo home.page.ts  
```
  arrayColeccionReparaciones: any = [{
    id: "",
    data: {} as Reparacion
   }];

  constructor(private firestoreService: FirestoreService) {
    this.obtenerListaReparaciones();
  }

  obtenerListaReparaciones(){
    this.firestoreService.consultar("reparaciones").subscribe((resultadoConsultaReparaciones) => {
      this.arrayColeccionReparaciones = [];
      resultadoConsultaReparaciones.forEach((datosReparacion: any) => {
        this.arrayColeccionReparaciones.push({
          id: datosReparacion.payload.doc.id,
          data: datosReparacion.payload.doc.data()
        });
      })
    });
  }
```

## Archivo home.page.html
```
<h1>LISTA DE REPARACIONES</h1>
  <ion-list>
    <ion-item *ngFor="let documentReparacion of arrayColeccionReparaciones">
      <ion-grid>
        <ion-row>
          <h2>{{documentReparacion.data.nombre}}</h2>
        </ion-row>
        <ion-row>
          <p>{{documentReparacion.data.fecha}}</p>
        </ion-row>
		<ion-row>
          <p>{{documentReparacion.data.lugar}}</p>
        </ion-row>
		<ion-row>
          <p>{{documentReparacion.data.precio}}</p>
        </ion-row>
		<ion-row>
          <p>{{documentReparacion.data.imagen}}</p>
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
 
 Creamos la propiedad idReparacionSelec para almacenar el Id que deseamos borrar. Pasamos el valos al método.borrar() anterior. El método reparacionSelec() se encarga de almacenar en idReparacionSelec el ID.
 
 ```
 idReparacionSelec: string;

  selecReparacion(reparacionSelec) {
    console.log("Reparacion seleccionada: ");
    console.log(reparacionSelec);
    this.idReparacionSelec = reparacionSelec.id;
    this.reparacionEditando.nombre = reparacionSelec.data.nombre;
    this.reparacionEditando.fecha = reparacionSelec.data.fecha;
    this.reparacionEditando.lugar = reparacionSelec.data.lugar;
    this.reparacionEditando.lugar = reparacionSelec.data.precio;
    this.reparacionEditando.imagen = reparacionSelec.data.imagen;
    this.router.navigate(['/detalle', this.idReparacionSelec]);
  }

  clicBotonBorrar() {
    this.firestoreService.borrar("reparaciones", this.idReparacionSelec).then(() => {
      // Actualizar la lista completa
      this.obtenerListaReparaciones();
      // Limpiar datos de pantalla
      this.reparacionEditando = {} as Reparacion;
    })
  }
 ```
 
## Archivo home.page.html
Añadir a cada item de la lista la llamada al método selecReparacion cuando se seleccione un determinado item. Se recorre el array arrayColeccionReparaciones, cargando en la variable documentReparacion cada uno de los elementos de ese array. Esa variable documentReparacion es la que se pasa como parámetro en la llamada a selecReparacion() que se ha implementado antes para almacenar el ID y los datos de la reparacion seleccionada por el usuario.
```
  <ion-list>
    <ion-item *ngFor="let documentReparacion of arrayColeccionReparaciones" (click)="selecReparacion(documentReparacion)">
      ...
    </ion-item>
  </ion-list>
```  
En este ejemplo se ha añadido un botón para eliminar el item que se encuentre seleccionado: 
`<ion-button (click)="clicBotonBorrar()">Borrar reparacion</ion-button>`

## Crear segunda página para el detalle
Con el siguiente comando, creamos una pantalla a la cual le damos el nombre de detalles.
`ionic g page detalle`

Podemos observa que en la carpeta src/app se ha creado una nueva subcarpeta (hermana de la carpeta home), donde se encuentran los archivos correspondientes a la segunda página que acabas de crear.
![](/assets/images/ionic-proyecto-final/24.PNG)

Además, en el archivo src/app/app-routing.module.ts verás que la segunda página ya ha sido agregada en el ruteo de la aplicación automáticamente:
![](/assets/images/ionic-proyecto-final/25.PNG)

## Enrutamiento usando Router de Angular
Deseamos que la segunda pantalla se abra al seleccionar un elemento de la lista que tenemos en la primera página. La sentencia que debes añadir a tu código (a continuación se indicará dónde) para que se abra la segunda pantalla es una llamada al método navigate de la clase Router:
![](/assets/images/ionic-proyecto-final/26.PNG)

Por tanto, esa línea deberás añadirla al método selecReparacion() que es el que se ejecuta cuando se hace clic en un elemento de la lista. Añádela al final, o al menos una vez que se haya obtenido el ID del registro seleccionado, porque será necesario conocer el ID para pasarlo después a la segunda página.

Al añadir esa línea verás que se obtiene un error al no estar declarada la variable router. Debes declarar esa variable como un objeto de la clase Router, y debe hacerse al inicio de la página, por lo que debes inyectarla en el constructor (si tienes otros parámetros en el constructor debes añadirle este nuevo parámetro separado por comas).
![](/assets/images/ionic-proyecto-final/27.PNG)

Como puedes ver, también se señala un error por no conocer la clase Router. Tendrás que importarla para corregirlo añadiendo en la parte superior un nuevo import:
![](/assets/images/ionic-proyecto-final/28.PNG)

Ya puedes probar que se abre la segunda página al pulsar sobre cualquier elemento de la lista.

## Paso de datos a la segunda página
Nuestro objetivo, de momento, va a ser que se muestre en la segunda pantalla el ID del elemento seleccionado. Por tanto se necesita pasar ese dato de la primera a la segunda pantalla.

Para pasar un dato de una pantalla a otra se debe declarar el nombre deseado para ese dato como un parámetro en el routing (app-routing.module.ts). Por ejemplo, si denominados id al parámetro, se debe añadir /:id al atributo path:
![](/assets/images/ionic-proyecto-final/29.PNG)

Ahora, para concretar el dato que se quiere pasar a la segunda página se debe añadir dicho dato en la llamada al método navigate que ya has añadido anteriormente. En nuestro ejemplo, se va a pasar la variable idReparacionSelec que está declarada como una propiedad de la clase.
![](/assets/images/ionic-proyecto-final/30.PNG)

## Recepción de los datos en la segunda página
La recepción del dato que proviene de la primera página se puede añadir en el método onInit() que se ejecuta automáticamente al iniciarse la segunda página. El dato obtenido se va a almacenar en una variable llamada también id, que es declarada como una propiedad de la clase para ampliar su visibilidad.
![](/assets/images/ionic-proyecto-final/31.PNG)

Observa que en el método get() se indica el nombre que se le había dado al parámetro en el routing (app-routing.module.ts) y en este caso se está guardando en una variable con el mismo nombre (id), pero se podría haber usado cualquier otra variable para almacenar el dato recibido.

## Mostrar el dato recibido
La información recibida desde la primera página ya se encontrará en la variable id de la segunda página, por lo que si deseas mostrar su contenido en la página HTML (de la segunda página), haz un DataBinding con los dobles corchetes donde desees mostrar el dato.
![](/assets/images/ionic-proyecto-final/32.PNG)

## Archivo firestore.service.ts
```
public consultarPorId(coleccion, documentId) {
  return this.angularFirestore.collection(coleccion).doc(documentId).snapshotChanges();
}
``` 
## Archivo xxxx.page.ts
Lo habitual puede ser que la consulta de los datos de un determinado documento por su ID se haga en un página distinta, por lo que no especifica aquí el nombre de la página donde colocar este código.
Los datos (un documento de FireStore) que se obtengan tras realizar la consulta se deben almacenar en alguna variable con una estructura que permita diferenciar el ID y los datos (DATA) obtenidos. En este ejemplo se va a crear la variable document con los campos id y data (de tipo Reparacion):
```
document: any = {
  id: "",
  data: {} as Reparacion
};
``` 

En el lugar del código que corresponda (donde se desee realizar la consulta por ID), se incluirán las siguientes líneas que realizan la consulta buscando el ID que se encuentre almacenado en la variable idConsultar (no se incluye en este ejemplo su declaración y asignación de valor, ya que depende del modo de uso que se haga de este tipo de consulta).
```
this.firestoreService.consultarPorId("reparaciones", this.id).subscribe((resultado) => {
      // Preguntar si se hay encontrado un document con ese ID
      if(resultado.payload.data() != null) {
        this.document.id = resultado.payload.id
        this.document.data = resultado.payload.data();
        // Como ejemplo, mostrar el nombre del cliente en consola
        console.log(this.document.data.nombre);
      } else {
        // No se ha encontrado un document con ese ID. Vaciar los datos que hubiera
        this.document.data = {} as Reparacion;
      } 
});
``` 

## Archivo xxxx.page.html
Para mostrar algún dato contenido en la consulta se deberá incluir algo como esto (muestra el nombre de la reparacion consultada):
`<p> Nombre: {{ document.data.nombre }} </p>`

Una vez que nos muestre el nombre, vamos a configurarlo para que nos muestre todos los datos.
```
<ion-item>
    <ion-label>Nombre Cliente </ion-label>
    <ion-input [(ngModel)]="document.data.nombre"></ion-input>
  </ion-item>
  <ion-item>
    <ion-label>Fecha prevista reparación</ion-label>
    <ion-input type="date" [(ngModel)]="document.data.fecha"></ion-input>
  </ion-item>
  <ion-item>
    <ion-label>Lugar de la reparación</ion-label>
    <ion-input [(ngModel)]="document.data.lugar"></ion-input>
  </ion-item>
  <ion-item>
    <ion-label>Precio de la reparación</ion-label>
    <ion-input [(ngModel)]="document.data.precio"></ion-input>
  </ion-item>
  <ion-item>
    <ion-label>Imágenes de la averia</ion-label>
    <ion-input [(ngModel)]="document.data.imagen"></ion-input>
  </ion-item>
``` 

El resultado seria el siguiente:
![](/assets/images/ionic-proyecto-final/33.PNG)

## Archivo detalle.page.html
Ahora vamos a ver como están los botones de añadir, modificar y borrar.
```
<ion-button color="light" (click)="clicBotonInsertar()" *ngIf="this.document.id == ''">
    <ion-icon name="add-circle-outline"></ion-icon>Añadir reparación</ion-button>
  <ion-list > 
    <ion-button color="light" (click)="clicBotonModificar()" *ngIf="this.document.id == this.id">
      <ion-icon name="layers-outline"></ion-icon>Modificar reparación</ion-button>
    <ion-button color="light" (click)="clicBotonBorrar()" *ngIf="this.document.id == this.id">
      <ion-icon name="trash-outline"></ion-icon>Borrar reparación</ion-button>
  </ion-list>
``` 

## Archivo detalle.page.ts
Ahora vamos a ver como están configurados los botones de añadir, modificar y borrar.
```
clicBotonModificar() {
    this.firestoreService.actualizar("reparaciones", this.document.id, this.document.data).then(() => {
      // Actualizar la lista completa
      this.obtenerListaReparaciones();
      console.log('Reparación modificada correctamente!');
      // Limpiar datos de pantalla
      this.reparacionEditando = {} as Reparacion;
    })
  }
``` 

```
clicBotonInsertar() {
    this.firestoreService.insertar("reparaciones", this.document.data).then(() => {
      console.log('Reparación creada correctamente!');
      this.reparacionEditando= {} as Reparacion;
    }, (error) => {
      console.error(error);
    });
  }
``` 

```
clicBotonBorrar() {
    this.firestoreService.borrar("reparaciones", this.document.id).then(() => {
              // Actualizar la lista completa
              this.obtenerListaReparaciones();
              console.log('Reparación borrada correctamente!');
              // Limpiar datos de pantalla
              this.reparacionEditando = {} as Reparacion;
 })
``` 

## Inserción en segunda página
Añade en la primera pantalla un botón de tipo ion-fab-button, de forma que quede anclado en la parte inferior de la pantalla. Este botón debe servir para añadir nuevos elementos a la lista. Cuando el usuario pulse este botón, se le llevará a la segunda pantalla (la misma que se ha usado anteriormente para mostrar el detalle del elemento seleccionado).
`<ion-fab vertical="bottom" horizontal="end" color="primary" slot="fixed">
      <ion-fab-button (click)="pasarSegudaPantalla()"><ion-icon name="add-circle-outline"></ion-icon></ion-fab-button>
    </ion-fab>`

El botón se veria de la siguiente forma:
![](/assets/images/ionic-proyecto-final/34.PNG)

Ahora vamos a ver el codigo para distinguir cuando se inserta una reparación a cuando se modifica o borra una reparación.
`*ngIf="this.document.id == ''"`
El código anterior significa que si el id está vacío es una nueva reparación, por lo que muestra el botón añadir.
`*ngIf="this.document.id == this.id"`
El código anterior significa que si el id existe, se modifica la reparación, por lo que muestra el botón modificar y borrar.

Como se muestra en los ejemplos siguientes:
![](/assets/images/ionic-proyecto-final/35.PNG)
![](/assets/images/ionic-proyecto-final/36.PNG)

## Confirmación de borrado
Añade una ventana de confirmación a la funcionalidad del botón de borrado, de manera que le pregunte al usuario si está seguro de hacer el borrado. Para ello, utiliza el componente ion-alert.
```
clicBotonBorrar() {
    this.alertController.create({
      header: 'ALERTA',
      subHeader: '¡Estas a punto de borrar una reparación!',
      message: 'Si deseas borrar una reparacion pulse si, en caso contrario pulse no',
      buttons: [
        {
          text: 'No',
          handler: () => {
            console.log('nunca');
          }
        },
        {
          text: 'Si',
          handler: () => {
            console.log('si');
            this.firestoreService.borrar("reparaciones", this.document.id).then(() => {
              // Actualizar la lista completa
              this.obtenerListaReparaciones();
              console.log('Reparación borrada correctamente!');
              // Limpiar datos de pantalla
              this.reparacionEditando = {} as Reparacion;
          })
        }
        }
      ]
    }).then(res => {
      res.present();
    });
    
  }
``` 

## Personalización de icono y splash
Prepara las imágenes
Crea una carpeta resources (si no existe) dentro del proyecto.
Crea o busca una imagen para el icono. Deberá ser una imagen PNG de tamaño 1024x1024 píxeles. Asígnale el nombre icon.png y almacénala en la carpeta resources.
Crea o busca una imagen para la pantalla de presentación (splash screen). Deberá ser una imagen PNG de tamaño 2732x2732 píxeles. Asígnale el nombre splash.png y almacénala en la carpeta resources.
Para aplicaciones a partir de Android 8.0 (nivel de API 26):
Haz una copia de esa misma imagen del icono pero de un tamaño 432x432 píxeles. Asígnale el nombre icon-foreground.png y almacénala en la carpeta resources/android (deberás crear esa carpeta android si no existe).
Crea o busca una imagen con las transparencias que desees para usar el icono de manera adaptativa (Designing Adaptive Icons). Debe ser una imagen de 432x432 píxeles. Asígnale el nombre icon-background.png y almacénala en la carpeta resources/android.
Deberá quedar una estructura similar a la siguiente:
![](/assets/images/ionic-proyecto-final/44.PNG)

## Instala cordova-res
Instala la aplicación cordova-res, si no lo has instalado previamente en el sistema:
`npm install -g cordova-res`

## Genera las imágenes con los tamaños adecuados
Ejecuta (en la carpeta del proyecto) los comandos siguientes que necesites según el tipo de plataforma (ios o android) que desees:
`cordova-res ios --skip-config --copy`
`cordova-res android --skip-config --copy`

De esa manera se habrán generado automáticamente los archivos correspondientes al icono y a la splash para multitud de posibles tamaños de pantalla.
![](/assets/images/ionic-proyecto-final/45.PNG)

## Compila e instala la aplicación en un dispositivo
Finalmente, compila e instala la aplicación en un dispositivo real o virtual para observar si se ha creado correctamente el icono y que aparece la splash screen al abrir la aplicación.

## Imágenes de demostración en distintas plataformas
![](/assets/images/ionic-proyecto-final/37.PNG)
![](/assets/images/ionic-proyecto-final/38.PNG)
![](/assets/images/ionic-proyecto-final/39.PNG)
![](/assets/images/ionic-proyecto-final/40.PNG)
![](/assets/images/ionic-proyecto-final/41.PNG)
![](/assets/images/ionic-proyecto-final/42.PNG)
![](/assets/images/ionic-proyecto-final/43.PNG)



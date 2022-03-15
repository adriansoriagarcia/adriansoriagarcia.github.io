---
layout: single
title: Proyecto final de Ionic con Firebase completo.
excerpt: "En este artículo vamos a ver el resultado final de una app multiplataforma de Cloud Firestore con Ionic"
date: 2022-03-15
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
* https://drive.google.com/file/d/1NA7q_OG0rNpSVYct_ctKhdn8gbBLFWN-/view?usp=sharing

## Imágenes de funcionamiento del proyecto

Pantalla home sin usuario logueado.
![](/assets/images/ionic-proyecto-manitas/1.jpg)

Pantalla home con usuario logueado.
![](/assets/images/ionic-proyecto-manitas/2.jpg)

Pantalla de formulario con sus correspondientes botones en el cual se puede añadir, eliminar imágenes, guardar las imagenes seleccionadas y añadir las reparaciones..
![](/assets/images/ionic-proyecto-manitas/3.jpg)

Pantalla de información con su botón para llamar y su mapa de localización.
![](/assets/images/ionic-proyecto-manitas/4.jpg)

Pantalla de login, en la cual tambien podemos registrarnos pulsando en "Create an account".
![](/assets/images/ionic-proyecto-manitas/5.jpg)

Pantalla de registro, en la cual una vez registrado, nos podemos dirigir a la página de login..
![](/assets/images/ionic-proyecto-manitas/6.jpg)

Vista de la página home desde el ordenador.
![](/assets/images/ionic-proyecto-manitas/7.PNG)

Vista de la página login desde el ordenador.
![](/assets/images/ionic-proyecto-manitas/8.PNG)

Vista de la página registro desde el ordenador.
![](/assets/images/ionic-proyecto-manitas/9.PNG)

Vista de la página formulario desde el ordenador.
![](/assets/images/ionic-proyecto-manitas/10.PNG)

## Archivo Home.html
```
<ion-header [translucent]="true">
  <ion-toolbar>
    <ion-title>
      Proyecto manitas
    </ion-title>
    <p class="usuario">User: {{ usuario }} </p>
    <ion-button class="login" (click)="login()" color="dark" *ngIf=" this.userUID == ''">
      <ion-icon name="log-in-outline"></ion-icon>
    </ion-button>
    <ion-button class="login" (click)="logout()" color="dark" *ngIf=" this.userUID != ''">
      <ion-icon name="log-out-outline"></ion-icon>
    </ion-button>
  </ion-toolbar>
</ion-header>

<ion-content [fullscreen]="true">
  <ion-searchbar showCancelButton="never" [(ngModel)]="filtro"></ion-searchbar>
  <h1>LISTA DE REPARACIONES</h1>
    <!--<ion-fab vertical="bottom" horizontal="end" color="primary" slot="fixed">
      <ion-fab-button (click)="pasarSegudaPantalla()"><ion-icon name="add-circle-outline"></ion-icon></ion-fab-button>
    </ion-fab>-->
      <ion-grid class="contenido">
      <ion-row>
        <ng-container *ngFor="let documentReparacion of arrayColeccionReparaciones">
        <ion-col *ngIf="filtro === '' || documentReparacion.data.nombre.includes(filtro)|| documentReparacion.data.fecha.includes(filtro) " size-xl="2" size-lg="2" size-md="4" size-sm="6" size-xs="12">
          <ion-card class="tarjetas"  (click)="selecReparacion(documentReparacion)">
            <ion-card-content>
                <h2>Nombre: {{documentReparacion.data.nombre}}</h2>
                <p>Fecha: {{documentReparacion.data.fecha}}</p>
                <p>Ubicación: {{documentReparacion.data.lugar}}</p>
                <p>Precio: {{documentReparacion.data.precio}} €</p>
                <img class="imagenes" src="{{documentReparacion.data.imagen}}">
            </ion-card-content>
          </ion-card>
        </ion-col>
      </ng-container>
      </ion-row>
    </ion-grid>  

    <ion-fab vertical="bottom" horizontal="left" slot="fixed">
      <ion-fab-button>
        <ion-icon name="share"></ion-icon>
      </ion-fab-button>
      <ion-fab-list side="end">
        <ion-fab-button href="https://es-es.facebook.com/"><ion-icon name="logo-facebook"></ion-icon></ion-fab-button>
      </ion-fab-list>
      <ion-fab-list side="top">
        <ion-fab-button href="https://www.instagram.com/?hl=es"><ion-icon name="logo-instagram"></ion-icon></ion-fab-button>
      </ion-fab-list>
    </ion-fab>

</ion-content>


<ion-toolbar>
  <ion-tabs>
    <ion-tab-bar >
      <ion-tab-button tab="anadir" *ngIf=" this.userUID != ''">
        <ion-icon name="add-circle-outline"></ion-icon>
        <ion-label>Añadir</ion-label>
      </ion-tab-button>

      <ion-tab-button tab="acerca">
        <ion-icon name="information-circle-outline"></ion-icon>
        <ion-label>Acerca de</ion-label>
      </ion-tab-button>
    </ion-tab-bar>
  </ion-tabs>
</ion-toolbar>
```

## Archivo Detalle.html
```
<ion-header>
  <ion-toolbar>
    <ion-buttons slot="start">
      <ion-button (click)="pasarPrimeraPantalla()" color="dark">
        <ion-icon name="arrow-back-outline"></ion-icon>
      </ion-button>
      <ion-title>Formulario</ion-title>
    </ion-buttons>
  </ion-toolbar>
</ion-header>

<ion-content [fullscreen]="true">
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
    <ion-label>Imagen</ion-label>
    <ion-button color="light" (click)="seleccionarImagen()">
      <ion-icon name="add"></ion-icon></ion-button>
    <ion-button color="light" (click)="borrarImagen()">
      <ion-icon name="close"></ion-icon></ion-button>
    <img class="imagenes" src="{{document.data.imagen}}" *ngIf="this.document.data.imagen != null">
  </ion-item>
  
  <ion-button color="light" (click)="guardarDatos()">
    <ion-icon name="save-outline"></ion-icon></ion-button>
  <ion-button color="light" (click)="clicBotonInsertar()" *ngIf="this.document.id == ''">
    <ion-icon name="add-circle-outline"></ion-icon></ion-button>
  <ion-list > 
    <ion-button color="light" (click)="clicBotonModificar()" *ngIf="this.document.id == this.id">
      <ion-icon name="layers-outline"></ion-icon></ion-button>
    <ion-button color="light" (click)="clicBotonBorrar()" *ngIf="this.document.id == this.id">
      <ion-icon name="trash-outline"></ion-icon></ion-button>
  </ion-list>
 
  <ion-col size="2">
    <ion-button color="light" (click)="ShareGeneric()" *ngIf="this.document.id == this.id">
      <ion-icon name="share-social-outline"></ion-icon></ion-button>
  </ion-col>
</ion-content>
```

## Archivo Informacion.html
```
<ion-header>
  <ion-toolbar>
    <ion-buttons slot="start">
      <ion-button (click)="pasarPrimeraPantalla()" color="dark">
        <ion-icon name="arrow-back-outline"></ion-icon>
      </ion-button>
      <ion-title>Información</ion-title>
    </ion-buttons>
  </ion-toolbar>
</ion-header>

<ion-content>
<h3>Atención al cliente: <ion-button (click)='llamada()'> 
  <ion-icon name="call-outline"></ion-icon>
</ion-button></h3>

<h3 class="local">Localización: </h3>
<div id="mapId" style="width:100%; height:50%; "></div>
</ion-content>
```

## Archivo Home.ts.
```
import { Component, ViewChild  } from '@angular/core';
import { FirestoreService } from '../firestore.service';
import { Reparacion } from '../reparacion';
import { Router } from '@angular/router';
import { AuthService } from '../services/auth.service';
import { LoadingController } from '@ionic/angular';
import { AngularFireAuth } from '@angular/fire/compat/auth';


@Component({
  selector: 'app-home',
  templateUrl: 'home.page.html',
  styleUrls: ['home.page.scss'],
})
export class HomePage {

  usuario: String = "";
  userEmail: String = "";
  userUID: String = "";
  isLogged: boolean;

  reparacionEditando: Reparacion; 
  filtro: string = '';
  
  arrayColeccionReparaciones: any = [{
    id: "",
    data: {} as Reparacion
   }];

   

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

  constructor(private firestoreService: FirestoreService, 
    private router:Router,
    public loadingCtrl: LoadingController,
    private authService: AuthService,
    public afAuth: AngularFireAuth) {
    // Crear una reparacion vacía
    this.obtenerListaReparaciones();
    this.reparacionEditando = {} as Reparacion;
    
  }

  idReparacionSelec: string;

  pasarSegudaPantalla () {
    this.router.navigate(['detalle/:id'])
  }


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

  ionViewDidEnter() {
    this.isLogged = false;
    this.afAuth.user.subscribe(user => {
      if(user){
        this.userEmail = user.email;
        var email_analizado = /^([^]+)@(\w+).(\w+)$/.exec(user.email);
        this.usuario=email_analizado[1];
        console.log(this.usuario)
        this.userUID = user.uid;
        this.isLogged = true;
      } else {
        this.router.navigate(["/home"]);
      }
    })
  }

  login() {
    this.router.navigate(["/login"]);
  }

  logout(){
    this.authService.doLogout()
    .then(res => {
      this.userEmail = "";
      this.userUID = "";
      this.usuario="";
      this.isLogged = false;
      console.log(this.userEmail);
    }, err => console.log(err));
  }



}

```

## ## Archivo Detalle.ts.
```
import { Component, OnInit } from '@angular/core';
import {ActivatedRoute } from '@angular/router';
import { Reparacion } from '../reparacion';
import { FirestoreService } from '../firestore.service';
import { AlertController } from '@ionic/angular';

import { Router } from '@angular/router';

import { LoadingController, ToastController } from '@ionic/angular';
import { ImagePicker } from '@awesome-cordova-plugins/image-picker/ngx';

import { SocialSharing } from '@awesome-cordova-plugins/social-sharing/ngx';
//import { SocialSharing } from '@ionic-native/social-sharing/ngx';

import { AngularFireAuth } from '@angular/fire/compat/auth';



@Component({
  selector: 'app-detalle',
  templateUrl: './detalle.page.html',
  styleUrls: ['./detalle.page.scss'],
})

export class DetallePage implements OnInit {

   // Imagen que se va a mostrar en la página
   imagenTempSrc: String;

   subirArchivoImagen: boolean = false;
   borrarArchivoImagen: boolean = false;

   // Nombre de la colección en Firestore Database
  reparacciones: String = "EjemploImagenes";

  reparacionEditando: Reparacion; 

  id:string="";
  imageURL: String;

  userEmail: String = "";
  userUID: String = "";
  isLogged: boolean;


  document: any = {
    id: "",
    data: {} as Reparacion,
  };

  arrayColeccionReparaciones: any = [{
    id: "",
    data: {} as Reparacion
   }];

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

  constructor(private activatedRoute: ActivatedRoute,
    private firestoreService: FirestoreService,
    public alertController: AlertController,
    private loadingController: LoadingController,
    private toastController: ToastController,
    private imagePicker: ImagePicker,
    private router:Router,
    private socialSharing: SocialSharing,
    public afAuth: AngularFireAuth
    ) {
    console.log(this.id)
    this.reparacionEditando = {} as Reparacion;
    this.obtenerListaReparaciones();
    
   };

   pasarPrimeraPantalla () {
    this.router.navigate(['home'])
  }


   clicBotonInsertar() {
    this.firestoreService.insertar("reparaciones", this.document.data).then(() => {
      console.log('Reparación creada correctamente!');
      this.reparacionEditando= {} as Reparacion;
      this.pasarPrimeraPantalla();
    }, (error) => {
      console.error(error);
    });
  }

  ngOnInit() {
    this.id = this.activatedRoute.snapshot.paramMap.get('id')
    this.firestoreService.consultarPorId("reparaciones", this.id).subscribe((resultado) => {
      // Preguntar si se hay encontrado un document con ese ID
      if(resultado.payload.data() != null) {
        this.document.id = resultado.payload.id
        this.document.data = resultado.payload.data();
        this.imagenTempSrc = this.document.data.imagen;
        // Como ejemplo, mostrar el nombre del cliente en consola
        console.log(this.document.data.imagen);
        //console.log(this.imageURL)
      } else {
        // No se ha encontrado un document con ese ID. Vaciar los datos que hubiera
        this.document.data = {} as Reparacion;
        //console.log(this.document.data.imagen)
      } 
    });
    
  }

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
            this.borrarImagen();
            this.guardarDatos();
            this.firestoreService.borrar("reparaciones", this.document.id).then(() => {
              // Actualizar la lista completa
              this.obtenerListaReparaciones(); 
              console.log('Reparación borrada correctamente!');
              // Limpiar datos de pantalla
              this.reparacionEditando = {} as Reparacion;
              this.pasarPrimeraPantalla();
          })
        }
        }
      ]
    }).then(res => {
      res.present();
    });
    
  }

  clicBotonModificar() {
    this.firestoreService.actualizar("reparaciones", this.document.id, this.document.data).then(() => {
      // Actualizar la lista completa
      this.obtenerListaReparaciones();
      console.log('Reparación modificada correctamente!');
      // Limpiar datos de pantalla
      this.reparacionEditando = {} as Reparacion;
      this.pasarPrimeraPantalla();
    })
  }

  async seleccionarImagen() {
    // Comprobar si la aplicación tiene permisos de lectura
    this.imagePicker.hasReadPermission().then(
      (result) => {
        // Si no tiene permiso de lectura se solicita al usuario
        if(result == false){
          this.imagePicker.requestReadPermission();
        }
        else {
          // Abrir selector de imágenes (ImagePicker)
          this.imagePicker.getPictures({
            maximumImagesCount: 1,  // Permitir sólo 1 imagen
            outputType: 1           // 1 = Base64
          }).then(
            (results) => {  // En la variable results se tienen las imágenes seleccionadas
              if(results.length > 0) { // Si el usuario ha elegido alguna imagen
                this.imagenTempSrc = "data:image/jpeg;base64,"+results[0];
                //this.document.data.imagen=this.imagenTempSrc;
                console.log("Imagen que se ha seleccionado (en Base64): " + this.imagenTempSrc);
                // Se informa que se ha cambiado para que se suba la imagen cuando se actualice la BD
                this.subirArchivoImagen = true;
                this.borrarArchivoImagen = false;
              }
            },
            (err) => {
              console.log(err)
            }
          );
        }
      }, (err) => {
        console.log(err);
      });
  }

  public guardarDatos() {
    if(this.subirArchivoImagen) {
      // Borrar el archivo de la imagen antigua si la hubiera
      if(this.document.data.imagen != null) {
        this.eliminarArchivo(this.document.data.imagen);        
      }
      // Si la imagen es nueva se sube como archivo y se actualiza la BD
      this.subirImagenActualizandoBD();
    } else {
      if(this.borrarArchivoImagen) {
        this.eliminarArchivo(this.document.data.imagen);        
        this.document.data.imagen = null;
      }
      // Si no ha cambiado la imagen no se sube como archivo, sólo se actualiza la BD
      this.actualizarBaseDatos();
    }
  }

  async subirImagenActualizandoBD(){
    // Mensaje de espera mientras se sube la imagen
    const loading = await this.loadingController.create({
      message: 'Please wait...'
    });
    // Mensaje de finalización de subida de la imagen
    const toast = await this.toastController.create({
      message: 'Image was updated successfully',
      duration: 3000
    });

    // Carpeta del Storage donde se almacenará la imagen
    let nombreCarpeta = "imagenes";

    // Mostrar el mensaje de espera
    loading.present();
    // Asignar el nombre de la imagen en función de la hora actual para
    //  evitar duplicidades de nombres         
    let nombreImagen = `${new Date().getTime()}`;
    // Llamar al método que sube la imagen al Storage
    this.firestoreService.subirImagenBase64(nombreCarpeta, nombreImagen, this.imagenTempSrc)
      .then(snapshot => {
        snapshot.ref.getDownloadURL()
          .then(downloadURL => {
            // En la variable downloadURL se tiene la dirección de descarga de la imagen
            console.log("downloadURL:" + downloadURL);
            //this.document.data.imagenURL = downloadURL;            
            // Mostrar el mensaje de finalización de la subida
            toast.present();
            // Ocultar mensaje de espera
            loading.dismiss();

            // Una vez que se ha termninado la subida de la imagen 
            //    se actualizan los datos en la BD
            this.document.data.imagen = downloadURL;
            this.actualizarBaseDatos();
          })
      })    
  } 

  public borrarImagen() {
    // No mostrar ninguna imagen en la página
    this.imagenTempSrc = null;
    // Se informa que no se debe subir ninguna imagen cuando se actualice la BD
    this.subirArchivoImagen = false;
    this.borrarArchivoImagen = true;
  }

  async eliminarArchivo(fileURL) {
    const toast = await this.toastController.create({
      message: 'File was deleted successfully',
      duration: 3000
    });
    this.firestoreService.borrarArchivoPorURL(fileURL)
      .then(() => {
        toast.present();
      }, (err) => {
        console.log(err);
      });
  }

  private actualizarBaseDatos() {    
    console.log("Guardando en la BD: ");
    console.log(this.document.data);
    this.firestoreService.actualizar(this.reparacciones, this.document.id, this.document.data);
  }

  text: string='Precio'
  link: string='https://ionicframework.com/'

  ShareGeneric(parameter){
    const url = this.link
    const text = this.text  + this.document.data.precio
    this.socialSharing.share(this.document.data.nombre, 'REPARACIÓN', null,  this.document.data.imagen)
  }

  ionViewDidEnter() {
    this.isLogged = false;
    this.afAuth.user.subscribe(user => {
      if(user){
        this.userEmail = user.email;
        this.userUID = user.uid;
        this.isLogged = true;
      } else {
        this.router.navigate(["/home"]);
      }
    })
  }

 
}

```

## Archivo Informacion.ts
```
import { Component, OnInit } from '@angular/core';

import { Router } from '@angular/router';

import { CallNumber } from '@awesome-cordova-plugins/call-number/ngx';

//import {Map, tileLayer} from 'leaflet';
import * as L from 'leaflet';

@Component({
  selector: 'app-informacion',
  templateUrl: './informacion.page.html',
  styleUrls: ['./informacion.page.scss'],
})
export class InformacionPage implements OnInit {

  map: L.Map;
  constructor(private router:Router,private callNumber:CallNumber) { }

  llamada(){
    this.callNumber.callNumber('3521234567', true)
    .then(() => console.log('Llamada exitosa!'))
    .catch(() => console.log('Error al intentar llamar'));
 
}

  pasarPrimeraPantalla () {
    this.router.navigate(['home'])
  }

  ionViewDidEnter(){
    this.loadMap();
  }

  loadMap() {
    var customIcon = new L.Icon({
      iconUrl: 'https://img.icons8.com/color/48/000000/google-maps-new.png',
      iconSize: [50, 50],
      iconAnchor: [25, 50]
    });
    let latitud = 36.922349;
    let longitud = -5.541855;
    let zoom = 17;
    this.map = L.map("mapId").setView([latitud, longitud], zoom);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png')
        .addTo(this.map);
    L.marker([ 36.922349,-5.541855],{icon: customIcon}).bindPopup("<p>Nos encontramos en la calle Torre Gailín Nº80</p>").addTo(this.map);
    
  }
  

  ngOnInit() {
    
  }

}

```
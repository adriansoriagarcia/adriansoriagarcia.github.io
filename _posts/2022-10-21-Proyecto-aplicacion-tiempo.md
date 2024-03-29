---
layout: single
title: Aplicación meteorologica.
excerpt: "En este artículo vamos a ver el resultado final de una pequeña aplicación web meteorologica con javascript vanilla."
date: 2022-10-21
classes: wide
header:
  teaser: /assets/images/htb-writeup-ready/ready_logo.png
  teaser_home_page: true
  icon: /assets/images/programacion.png
categories:
  - programación
  - JavaScript
  
tags:
  - proyectos
  - web
---

Aquí adjunto el enlace de mi github para poder descargar el proyecto = 
* https://github.com/adriansoriagarcia/appWeather

## ¿En qué consiste la aplicación?

La aplicación tiene dos partes:

* Primera: Muestra la temperatura actual, viento, humedad, visibilidad y presión.

* Segunda: TMuestra varias imágenes en las cuales se puede ver el tiempo para varios días.

## Imagenes de la aplicación.

En la siguiente imágen podemos ver como es la primera parte de la aplicación.
![](/assets/images/aplicacion-tiempo/1.PNG)

En la siguiente imágen podemos ver como es la segunda parte de la aplicación.
![](/assets/images/aplicacion-tiempo/2.PNG)

## Código Html
```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="description" content="">
    <meta name="author" content="Mark Otto, Jacob Thornton, and Bootstrap contributors">
    <meta name="generator" content="Hugo 0.101.0">
    <title>Weather example · Bootstrap v5.2</title>
    <link rel="icon" href="../assets/brand/cloud-fill.svg">
    <link href="../assets/dist/css/bootstrap.min.css" rel="stylesheet">
    <script src="../assets/dist/js/index.js"></script>

    <style>
      
      .bd-placeholder-img {
        font-size: 1.125rem;
        text-anchor: middle;
        -webkit-user-select: none;
        -moz-user-select: none;
        user-select: none;
      }

      @media (min-width: 768px) {
        .bd-placeholder-img-lg {
          font-size: 3.5rem;
        }
      }

      .b-example-divider {
        height: 3rem;
        background-color: rgba(0, 0, 0, .1);
        border: solid rgba(0, 0, 0, .15);
        border-width: 1px 0;
        box-shadow: inset 0 .5em 1.5em rgba(0, 0, 0, .1), inset 0 .125em .5em rgba(0, 0, 0, .15);
      }

      .b-example-vr {
        flex-shrink: 0;
        width: 1.5rem;
        height: 100vh;
      }

      .bi {
        vertical-align: -.125em;
        fill: currentColor;
      }

      .nav-scroller {
        position: relative;
        z-index: 2;
        height: 2.75rem;
        overflow-y: hidden;
      }

      .nav-scroller .nav {
        display: flex;
        flex-wrap: nowrap;
        padding-bottom: 1rem;
        margin-top: -1px;
        overflow-x: auto;
        text-align: center;
        white-space: nowrap;
        -webkit-overflow-scrolling: touch;
      }

      main{
        background-color: #1E213A;
      }


      #contenido{
        background-color: #100E1D;
      }

      #temperatura{
        color: white;
      }

      #datos{
        background-color: aqua;
      }

      #localizacion{
        width:50% ;
      }

    </style>

    
  </head>
  <body>
    
<header>
  <div class="collapse bg-dark" id="navbarHeader">
    <div class="container">
      <div class="row">
        <div class="col-sm-8 col-md-7 py-4">
          <h4 class="text-white">About</h4>
          <p class="text-muted"> Aplicación meteorologica en la cual te muestra la temperatura actual, viento, humedad, visibilidad y presión, ademas de el tiempo para varios dias de la semana.</p>
        </div>
        <div class="col-sm-4 offset-md-1 py-4">
          <h4 class="text-white">Contact</h4>
          <ul class="list-unstyled">

            <li><a href="https://www.instagram.com/" class="text-white">
              <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="currentColor" class="bi bi-instagram" viewBox="0 0 16 16">
                <path d="M8 0C5.829 0 5.556.01 4.703.048 3.85.088 3.269.222 2.76.42a3.917 3.917 0 0 0-1.417.923A3.927 3.927 0 0 0 .42 2.76C.222 3.268.087 3.85.048 4.7.01 5.555 0 5.827 0 8.001c0 2.172.01 2.444.048 3.297.04.852.174 1.433.372 1.942.205.526.478.972.923 1.417.444.445.89.719 1.416.923.51.198 1.09.333 1.942.372C5.555 15.99 5.827 16 8 16s2.444-.01 3.298-.048c.851-.04 1.434-.174 1.943-.372a3.916 3.916 0 0 0 1.416-.923c.445-.445.718-.891.923-1.417.197-.509.332-1.09.372-1.942C15.99 10.445 16 10.173 16 8s-.01-2.445-.048-3.299c-.04-.851-.175-1.433-.372-1.941a3.926 3.926 0 0 0-.923-1.417A3.911 3.911 0 0 0 13.24.42c-.51-.198-1.092-.333-1.943-.372C10.443.01 10.172 0 7.998 0h.003zm-.717 1.442h.718c2.136 0 2.389.007 3.232.046.78.035 1.204.166 1.486.275.373.145.64.319.92.599.28.28.453.546.598.92.11.281.24.705.275 1.485.039.843.047 1.096.047 3.231s-.008 2.389-.047 3.232c-.035.78-.166 1.203-.275 1.485a2.47 2.47 0 0 1-.599.919c-.28.28-.546.453-.92.598-.28.11-.704.24-1.485.276-.843.038-1.096.047-3.232.047s-2.39-.009-3.233-.047c-.78-.036-1.203-.166-1.485-.276a2.478 2.478 0 0 1-.92-.598 2.48 2.48 0 0 1-.6-.92c-.109-.281-.24-.705-.275-1.485-.038-.843-.046-1.096-.046-3.233 0-2.136.008-2.388.046-3.231.036-.78.166-1.204.276-1.486.145-.373.319-.64.599-.92.28-.28.546-.453.92-.598.282-.11.705-.24 1.485-.276.738-.034 1.024-.044 2.515-.045v.002zm4.988 1.328a.96.96 0 1 0 0 1.92.96.96 0 0 0 0-1.92zm-4.27 1.122a4.109 4.109 0 1 0 0 8.217 4.109 4.109 0 0 0 0-8.217zm0 1.441a2.667 2.667 0 1 1 0 5.334 2.667 2.667 0 0 1 0-5.334z"/>
              </svg>
            </a></li>

            <li><a href="https://github.com/adriansoriagarcia" class="text-white">
              <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="currentColor" class="bi bi-github" viewBox="0 0 16 16">
                <path d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.012 8.012 0 0 0 16 8c0-4.42-3.58-8-8-8z"/>
              </svg>
            </a></li>

            <li><a href="https://outlook.live.com/owa/" class="text-white">
              <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="currentColor" class="bi bi-mailbox" viewBox="0 0 16 16">
                <path d="M4 4a3 3 0 0 0-3 3v6h6V7a3 3 0 0 0-3-3zm0-1h8a4 4 0 0 1 4 4v6a1 1 0 0 1-1 1H1a1 1 0 0 1-1-1V7a4 4 0 0 1 4-4zm2.646 1A3.99 3.99 0 0 1 8 7v6h7V7a3 3 0 0 0-3-3H6.646z"/>
                <path d="M11.793 8.5H9v-1h5a.5.5 0 0 1 .5.5v1a.5.5 0 0 1-.5.5h-1a.5.5 0 0 1-.354-.146l-.853-.854zM5 7c0 .552-.448 0-1 0s-1 .552-1 0a1 1 0 0 1 2 0z"/>
              </svg>
            </a></li>
          </ul>
        </div>
      </div>
    </div>
  </div>
  <div class="navbar navbar-dark bg-dark shadow-sm">
    <div class="container">
      <a href="../index.html" class="navbar-brand d-flex align-items-center">
        <svg xmlns="http://www.w3.org/2000/svg" width="22" height="22" fill="currentColor"  class="bi bi-cloud-fill" viewBox="0 0 18 13">
          <path d="M4.406 3.342A5.53 5.53 0 0 1 8 2c2.69 0 4.923 2 5.166 4.579C14.758 6.804 16 8.137 16 9.773 16 11.569 14.502 13 12.687 13H3.781C1.708 13 0 11.366 0 9.318c0-1.763 1.266-3.223 2.942-3.593.143-.863.698-1.723 1.464-2.383z"/>
        </svg>
        <strong>Weather</strong>
      </a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarHeader" aria-controls="navbarHeader" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
    </div>
  </div>
</header>

<main>
  <section class="text-center container">
    <div class="row py-lg-2">
      
      <div class="col-lg-6 col-md-8 mx-auto">
        <a id="localizacion" href="#" class="btn btn-primary disabled ">Ubicación</a><br>
        <img class="tiempo" id="tiempo" alt="tiempo" src="../assets/img/Sleet.png">
        <h1 id="temperatura" class="fw-light">Temperatura</h1>
        <p>
          <a id="estado" href="#" class="btn btn-success disabled">Estado</a>
        </p>
      </div>
    </div>


    <div class="album py-5 bg-light">
      <div class="container">
        <div class="row row-cols-1 row-cols-sm-2 row-cols-md-2 g-3">
          <div class="col">
            <div class="card shadow-sm">
              <svg class="bd-placeholder-img card-img-top" width="25%" height="25%" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Placeholder: Thumbnail" preserveAspectRatio="xMidYMid slice" focusable="false"><title>Wind status</title><rect width="100%" height="100%" fill="#1E213A"/><text id="viento" x="50%" y="50%" fill="#eceeef" dy=".3em">Wind status </text></svg>
            </div>
          </div>
          <div class="col">
            <div class="card shadow-sm">
              <svg class="bd-placeholder-img card-img-top" width="25%" height="25%" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Placeholder: Thumbnail" preserveAspectRatio="xMidYMid slice" focusable="false"><title>Humidity</title><rect width="100%" height="100%" fill="#1E213A"/><text id="humedad" x="50%" y="50%" fill="#eceeef" dy=".3em">Humidity</text></svg>
            </div>
          </div>
  
          <div class="col">
            <div class="card shadow-sm">
              <svg class="bd-placeholder-img card-img-top" width="25%" height="25%" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Placeholder: Thumbnail" preserveAspectRatio="xMidYMid slice" focusable="false"><title>Visibility</title><rect width="100%" height="100%" fill="#1E213A"/><text id="visibilidad" x="50%" y="50%" fill="#eceeef" dy=".3em">Visibility</text></svg>
            </div>
          </div>
          <div class="col">
            <div class="card shadow-sm">
              <svg class="bd-placeholder-img card-img-top" width="25%" height="25%" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Placeholder: Thumbnail" preserveAspectRatio="xMidYMid slice" focusable="false"><title>Air Pressure</title><rect width="100%" height="100%" fill="#1E213A"/><text id="presion" x="50%" y="50%" fill="#eceeef" dy=".3em">Air Pressure</text></svg>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <div id="contenido" class="album py-5 bg-light">
    
    <div class="container text-center">
      <img id="semana" alt="image" style="width:60% ;">
    </div>
    
  </div>
  
</main>

<footer class="text-muted py-5">
  <div class="container">
    <p class="float-end mb-1">
      <a href="#" style="color: black;">
        <svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" fill="currentColor" class="bi bi-arrow-bar-up" viewBox="0 0 16 16">
          <path fill-rule="evenodd" d="M8 10a.5.5 0 0 0 .5-.5V3.707l2.146 2.147a.5.5 0 0 0 .708-.708l-3-3a.5.5 0 0 0-.708 0l-3 3a.5.5 0 1 0 .708.708L7.5 3.707V9.5a.5.5 0 0 0 .5.5zm-7 2.5a.5.5 0 0 1 .5-.5h13a.5.5 0 0 1 0 1h-13a.5.5 0 0 1-.5-.5z"/>
        </svg>
      </a>
    </p>
    <p class="mb-1">Aplicación web en fase de desarrollo.</p>
    <p class="mb-0">Created by Adrián</p>
  </div>
</footer>


    <script src="../assets/dist/js/bootstrap.bundle.min.js"></script>

      
  </body>
</html>

```

## Código JavaScript
```
function getLocation() {
    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(showPosition);
    } else {
        alert( "El navegador no soporta geolocalización.");
    }
}

function showPosition(position) {
        console.log("Latitud: " + position.coords.latitude +
        "\nLongitud: " + position.coords.longitude);
        consulatrAPI(position);
}

function consulatrAPI(position){

    //API básica = https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${appId}

    //console.log(position);
    const appId = '9ef0f6cd66c367ab044dec44ca6bd9ff';

    let lat = position.coords.latitude;
    let lon = position.coords.longitude;

    let url = `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${appId}`;

    var imagenes=new Array(
      [`https://www.7timer.info/bin/astro.php?lon=${lon}&lat=${lat}&ac=0&lang=es&unit=metric&output=internal&tzshift=0`],
      [`https://www.7timer.info/bin/meteo.php?lon=${lon}&lat=${lat}&ac=0&lang=es&unit=metric&output=internal&tzshift=0`],
      [`https://www.7timer.info/bin/civil.php?lon=${lon}&lat=${lat}&ac=0&lang=es&unit=metric&output=internal&tzshift=0`],
      [`https://www.7timer.info/bin/civillight.php?lon=${lon}&lat=${lat}&ac=0&lang=es&unit=metric&output=internal&tzshift=0`],
      [`https://www.7timer.info/bin/two.php?lon=${lon}&lat=${lat}&ac=0&lang=es&unit=metric&output=internal&tzshift=0`]
    );

    // fetch api 1 día
    fetch(url)
      .then(respuesta => {
        return respuesta.json();
      })
      .then(datos => {
        console.log(datos);
        //limpiarHTML();
        if(datos.cod === "404") {
          console.log('Ciudad No Encontrada');
        } else {
          mostrarClima(datos)
        }
      })
      .catch(error => {
        console.log(error)
      });


    climaSemana(imagenes);
}

function mostrarClima(datos){

    const { name, main: { temp} } = datos;
    const grados = (temp - 273.15).toFixed(0);
    const {  main: { weather } } = datos;
    const tiempo = datos.weather[0].main;
    const ubi = datos.name;

    const actual = document.getElementById("temperatura");
    actual.innerHTML = grados + "ºC";

    const estado = document.getElementById("estado");
    estado.innerHTML = tiempo;

    const ubicacion = document.getElementById("localizacion");
    ubicacion.innerHTML = ubi;

    //console.log(datos.main.pressure);

    document.getElementById("viento").innerHTML= "Viento: " + datos.wind.speed + " km/h";
    document.getElementById("humedad").innerHTML= "Humedad: " + datos.main.humidity + " %";

    let visibilidad = datos.visibility / 1000;

    document.getElementById("visibilidad").innerHTML= "Visibilidad: " +  visibilidad + " km";

    document.getElementById("presion").innerHTML="Presion: " + datos.main.pressure + " mb";

    if(tiempo == "Clear") {
        document.getElementById("tiempo").src="../assets/img/Clear.png";
    } else if (tiempo == "Clouds") {
        document.getElementById("tiempo").src="../assets/img/LightCloud.png";
    } else if (tiempo == "Rain"){
        document.getElementById("tiempo").src="../assets/img/LightRain.png";
    }


}

function climaSemana(imagenes){
  console.log(imagenes);
  //document.getElementById("semana").src = imagenes[2];

  let contador=0;
 
  /*
      Funcion para cambiar la imagen
   */
  function rotarImagenes()
  {
      // cambiamos la imagen 
      
      contador++
      if (contador > 4) {
        contador = 0;
      }
      document.getElementById("semana").src=imagenes[contador];

  }
 
      // Cargamos una imagen aleatoria
      rotarImagenes();

      // Indicamos que cada 30 segundos cambie la imagen
      setInterval(rotarImagenes,30000);
  
  
}

window.addEventListener("load",()=>getLocation());
```




---
layout: single
title: Proyecto final de JavaFXII.
excerpt: "En este artículo vamos a ver el resultado final de un juego creado en JavaFX"
date: 2022-03-18
classes: wide
header:
  teaser: /assets/images/htb-writeup-ready/ready_logo.png
  teaser_home_page: true
  icon: /assets/images/programacion.png
categories:
  - programación
  - JavaFX
  
tags:
  - proyectos
  - juego
  - bucles
---

Aquí adjunto el enlace de mi github para poder descargar el proyecto = 
* https://github.com/adriansoriagarcia/proyectoFinalJavaFXII.git

Aquí adjunto el enlace de un video de demostracion del juego = 
* https://drive.google.com/file/d/1R0F0KILuYLcjdEBsTNCxkwt8aMEStU9F/view?usp=sharing

## ¿En qué consiste el juego?

El juego consiste en buscar parejas de cartas antes de que se acaben los intentos. Tenemos tres niveles:
Nivel facíl: Tenemos 20 intentos.
Nivel medio: Tenemos 15 intentos.
Nivel dificil: Tenemos 10 intentos.

## Fín de partida

La partida termina cuando el jugador se ha quedado sin intentos o también cuando conseguimos emparejar todas las cartas.

## Reinicio

El jugador tiene la posibilidad de reiniciar la partida una vez terminada, ya sea por haber perdido o haber ganado.

## Imagenes del juego.

Pantalla principal en la cual podemos ver de que trata el juego y quien lo ha desarrollado, ademas de un botón para continuar.
![](/assets/images/JavaFXII-proyecto-final/1.PNG)

Cuando pulsamos el botón de continuar, podemos ver como es el juego, en el cual podemos elegir el nivel de dificultad, en el que dependiendo de dicho nivel tenemos diferentes intentos. Tambien podemos elegir si queremos sonido o no y cuando estemos listo, pulsaremos iniciar.
![](/assets/images/JavaFXII-proyecto-final/2.PNG)

Así se vera el juego una vez iniciado durante 4 segundos, que es el tiempo que nos muestra las cartas para que el jugador pueda memorizarlas.
![](/assets/images/JavaFXII-proyecto-final/3.PNG)

Sí el jugador agota todos los intentos, d una ventana el la cual se indica que 'Has perdido'.
![](/assets/images/JavaFXII-proyecto-final/4.PNG)

Por el contrario, si el jugador consigue emparejar todas las cartas, muestra una ventana en la cual se indica que 'Has ganado'.
![](/assets/images/JavaFXII-proyecto-final/5.PNG)

## Algunos de los métodos y clases utilizados:

* scene
* Panel
* VBox
* Timeline
* ChoiceBox
* Text
* ImageView
* Image
* Label
* AudioClip
* GridPane

## Clases utilizadas:
* App
* Control
* MenuPrincipal
* Carta
* Tablero
* PanelLateral

## Código Clase App
```
package es.adriansoriagarcia.proyectofinaljavafxii;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.layout.Pane;
import javafx.stage.Stage;


/**
 * JavaFX App
 */
public class App extends Application {

    @Override
    public void start(Stage stage) {
        // Panel principal que contendrá los elementos de la pantalla
        Pane paneRoot;
        final short tamXPantalla=750;//Constante con el tamaño horizontal de la pantalla
        final short tamYPantalla=580;//Constante con el tamaño vertical de la pantalla
        
        paneRoot = new Pane();
        //paneRoot.setAlignment(Pos.CENTER);
        var scene = new Scene(paneRoot, tamXPantalla, tamYPantalla);
        stage.setScene(scene);
        stage.show();
        stage.setTitle("Memoria");
        stage.setResizable(false);
        
        MenuPrincipal menu = new MenuPrincipal();
        paneRoot.getChildren().add(menu);
        
        //Añadir imagen de fondo.
        paneRoot.setStyle("-fx-background-image:url('images/background.jpg');");
        
        
        
    }

    public static void main(String[] args) {
        launch();
    }

}
```

## Código Clase Control
```
package es.adriansoriagarcia.proyectofinaljavafxii;

import java.util.Random;

/**
 *
 * @author adrián
 */
public class Control {
    static short tamXTablero; //Declaración variable tamaño X tablero
    static short tamYTablero; //Declaración variable tamaño Y tablero          
    static int [][] tablero; //Declaración array tablero
    static char [][] encontrado;//Declaración array encontrado
    final char VACIO= 0;
    static final char SIPAREJA= 'S';
    static final char NOPAREJA= 'N';
    int aleatorioFilas;//Variable
    int aleatorioColumnas;//Variable
    int aleatorioFilas1;//Variable
    int aleatorioColumnas1;//Variable
    boolean finPartida = false;


    //Método constructor
    public Control(){
        tamXTablero = 4;
        tamYTablero = 4;
        int canNumeros= 9;
        int numRepeticiones=2;
        tablero = new int [tamXTablero][tamYTablero];
        encontrado = new char [tamXTablero][tamYTablero];
        Random random = new Random();
        
        //Bloque para generación de aleatorios array tablero
        for(int i=1; i<canNumeros; i++){
            for(int x=0; x<numRepeticiones; x++){
                aleatorioFilas = random.nextInt(tamXTablero);
                aleatorioColumnas = random.nextInt(tamYTablero);
                if(tablero[aleatorioColumnas][aleatorioFilas] == VACIO){
                    tablero[aleatorioColumnas][aleatorioFilas]= i;
                } else {
                    do {
                        aleatorioColumnas1 = random.nextInt(tamXTablero);
                        aleatorioFilas1 = random.nextInt(tamYTablero);

                    }
                    while(tablero[aleatorioColumnas1][aleatorioFilas1] != VACIO );
                    tablero[aleatorioColumnas1][aleatorioFilas1]=i;
                }
            }
        } 
        
        //Bloque para generación array parejas encontradas
        for(int x=0; x<tamXTablero;x++){
            for(int y=0; y<tamYTablero; y++){
                encontrado[x][y] = NOPAREJA; 
            }
        }
        
    }
    
    public Control(short tamX, short tamY){
        tamXTablero = tamX;
        tamYTablero = tamY;  
        int canNumeros= tamXTablero * tamYTablero / 2 + 1;
        System.out.println(canNumeros);
        int numRepeticiones=2;
        tablero = new int [tamXTablero][tamYTablero];
        encontrado = new char [tamXTablero][tamYTablero];
        Random random = new Random();
        
        //Bloque para generación de aleatorios array tablero
        for(int i=1; i<canNumeros; i++){
            for(int x=0; x<numRepeticiones; x++){
                aleatorioFilas = random.nextInt(tamXTablero);
                aleatorioColumnas = random.nextInt(tamYTablero);
                if(tablero[aleatorioColumnas][aleatorioFilas] == VACIO){
                    tablero[aleatorioColumnas][aleatorioFilas]= i;
                } else {
                    do {
                        aleatorioColumnas1 = random.nextInt(tamXTablero);
                        aleatorioFilas1 = random.nextInt(tamYTablero);

                    }
                    while(tablero[aleatorioColumnas1][aleatorioFilas1] != VACIO );
                    tablero[aleatorioColumnas1][aleatorioFilas1]=i;
                }
            }
        } 
        
        //Bloque para generación array parejas encontradas
        for(int x=0; x<tamXTablero;x++){
            for(int y=0; y<tamYTablero; y++){
                encontrado[x][y] = NOPAREJA; 
            }
        }
        
    }
   
    
    public void mostrarTableroConsola(){
        for(int y=0; y<tamYTablero; y++){   
            for(int x=0; x<tamXTablero;x++){
                System.out.print(encontrado[x][y]);      
            }
            System.out.println();
        }
        System.out.println();
        for(int y=0; y<tamYTablero; y++){   
            for(int x=0; x<tamXTablero;x++){
                System.out.print(tablero[x][y]);      
            }
            System.out.println();
        }
        System.out.println();
    }
    
    public boolean buscarPareja(int posXcarta1,int posYcarta1, int posXcarta2,  int posYcarta2) {     
       //retorna true si coninciden las parejas de cartas o false si no coincide
        //System.out.println("Xcarta1 " + posXcarta1);
        //System.out.println("Ycarta1 " + posYcarta1);
        //System.out.println("Xcarta2 " + posXcarta2);
        //System.out.println("Ycarta2 " + posYcarta2);
        System.out.println("carta1 " + tablero[posXcarta1][posYcarta1]);
        System.out.println("carta2 " + tablero[posXcarta2][posYcarta2]);
        mostrarTableroConsola();
       if(tablero[posXcarta1][posYcarta1] == tablero[posXcarta2][posYcarta2]){
            encontrado[posXcarta1][posYcarta1] = SIPAREJA; 
            encontrado[posXcarta2][posYcarta2] = SIPAREJA;
           return true;
           
       }
       return false;
    }  
    
    
    public boolean finPartida() {
        for(int x=0; x<tamXTablero; x++) {
            for(int y=0; y<tamYTablero; y++) {
                if(encontrado[x][y] == NOPAREJA) {
                    return false; 
                    
                }
            }
        }
        finPartida = true;
        return true;        
    }
}
```

## Código Clase MenuPrincipal
```
package es.adriansoriagarcia.proyectofinaljavafxii;

import javafx.scene.control.Button;
import javafx.scene.layout.Pane;
import javafx.scene.text.Font;
import javafx.scene.text.FontPosture;
import javafx.scene.text.FontWeight;
import javafx.scene.text.Text;
import javafx.scene.text.TextAlignment;

/**
 *
 * @author usuario
 */
public class MenuPrincipal extends Pane{
    Button ButtonInicio;
    public MenuPrincipal(){
        ButtonInicio = new Button("Continuar");
        ButtonInicio.setLayoutX(300);
        ButtonInicio.setLayoutY(250);
        ButtonInicio.setMinSize(150,100);
        
        Text text = new Text();
        text.setFont(Font.font("verdana", FontWeight.BOLD, FontPosture.REGULAR, 15));
        text.setTextAlignment(TextAlignment.CENTER);
        text.setLayoutY(50);
        text.setLayoutX(10);
        text.setText("Juego de memoria es un juego de mesa con una baraja de cartas específicas."
                + "\n El objetivo consiste en encontrar los pares con la misma figura impresa utilizando\n la memoria.");
        
        Text nombre = new Text();
        nombre.setLayoutX(210);
        nombre.setLayoutY(550);
        nombre.setText("Desarrollado por: Adrián Soria García.");
        nombre.setFont(Font.font("verdana", FontWeight.BOLD, FontPosture.REGULAR, 15));
        nombre.setTextAlignment(TextAlignment.CENTER);
        
        this.getChildren().add(ButtonInicio);
        this.getChildren().add(text);
        this.getChildren().add(nombre);
        
        ButtonInicio.setOnAction((t) -> {
            Iniciar();
            ButtonInicio.setVisible(false);
            text.setVisible(false);
            nombre.setVisible(false);
            
        });
        
        
    }
    
    public void Iniciar(){
        Tablero tablero = new Tablero();
        tablero.setLayoutX(10);
        tablero.setLayoutY(10);
        this.getChildren().add(tablero);
        
        PanelLateral panel = new PanelLateral();
        panel.setLayoutX(600);
        panel.setLayoutY(140);
        this.getChildren().add(panel);
    }
    
}
```

## Código Clase Carta
```
package es.adriansoriagarcia.proyectofinaljavafxii;

import javafx.scene.image.Image;
import javafx.scene.image.ImageView;


public class Carta extends ImageView {

    static final int TAM_CARTA=140;
    //Método constructor Casilla
    /**
    * Selecciona la imágen dependiendo del numcarta correspondiente
    * @param numCarta es el numero correspondiente a su nombre
    */
    public Carta(byte numCarta){
        //System.out.println(numCarta);
        Image carta = new Image(getClass().getResourceAsStream("/images/"+numCarta+".PNG"));
        this.setImage(carta);
    }
     
}
```

## Código Clase Tablero
```
package es.adriansoriagarcia.proyectofinaljavafxii;

import static es.adriansoriagarcia.proyectofinaljavafxii.PanelLateral.textClick;
import static es.adriansoriagarcia.proyectofinaljavafxii.PanelLateral.textTiempo;

import java.net.URISyntaxException;
import java.net.URL;
import javafx.animation.KeyFrame;
import javafx.animation.Timeline;
import javafx.event.ActionEvent;
import javafx.geometry.Pos;
import javafx.scene.control.Alert;
import javafx.scene.control.Button;
import javafx.scene.control.DialogPane;
import javafx.scene.image.Image;
import javafx.scene.image.ImageView;
import javafx.scene.layout.GridPane;
import javafx.scene.media.AudioClip;
import javafx.scene.paint.Color;
import javafx.util.Duration;




/**
 *
 * @author adriÃ¡n
 */

//Recorre el array de numeros y dependiendo del valor de cada posiciÃ³n coloca una imagen en grindpanel
public class Tablero extends GridPane {
    
    public boolean play = false;
    byte c=0;
    byte columna;
    byte fila;
    byte columnaTemporal;
    byte filaTemporal;
    byte columna1;
    byte fila1;
    Control control;
    Carta carta;
    Timeline timelineocultaImagen;
    Timeline timelineImagen;
    Timeline timelineCuentaAtras;
    Timeline timelineEsperaFin;
    Image cartaBackground;
    ImageView imgView;
    ImageView [][] imageOculta;
    boolean controlAcierto;
    Button ButtonInicio;
    int valor;
    static short intentosRestantes = 15;
    byte cuentaAtras = 4;
    final int TEXT_SIZE = 24;
    int selectedNivel;//Indice del nivel de dificultad
    Alert AlertFin ;
    AudioClip audioClip1;
    URL urlAudio;
    
    /*
     * MÃ©todo constructor de la clase Tablero.
    */
    public Tablero() { 
        //this.setMinWidth(Carta.TAM_CARTA * Control.tamXTablero);
        //this.setMinHeight(Carta.TAM_CARTA * (Control.tamYTablero + 1)); 
        //this.setMaxWidth(Carta.TAM_CARTA * Control.tamXTablero);
        //this.setMaxHeight(Carta.TAM_CARTA * (Control.tamYTablero + 1)); 
        //this.setPrefWidth(Carta.TAM_CARTA * Control.tamXTablero);
        //this.setPrefHeight(Carta.TAM_CARTA * (Control.tamYTablero + 1)); 
        this.setAlignment(Pos.CENTER);
        control= new Control();
        //panel= new PanelLateral();
 
        
        PanelLateral.ButtonInicio = new Button("Iniciar");
        PanelLateral.btnStart=new Button("");
        PanelLateral.btnPause = new Button("");
        //----------------------------------------------------------------------
        //array de tamaÃ±o del tablero
        imageOculta = new ImageView [Control.tamXTablero][Control.tamYTablero];
        ocultarImagenes();
        muestraImagenesInicio(control);
        //this.getStylesheets().add("stylesheet.css");
        //urlAudio = getClass().getResource("/audio/carta.mp3");
        
        PanelLateral.btnStart.setOnAction((t) -> {
            urlAudio = getClass().getResource("/audio/carta.mp3");
        });
        PanelLateral.btnPause.setOnAction((t) -> {
            urlAudio = getClass().getResource("/audio/silencio.mp3");
        });
        
    } 
    /*
     * Muestra las imÃ¡genes de todo el tablero durante x segundos al iniciar.
    */
    private void muestraImagenesInicio(Control control){
        PanelLateral.ButtonInicio.setOnAction((t) -> {
            anadeImagenes();
            timelineCuentaAtras.play();
            timelineImagen.play();
            textClick.setText(String.valueOf(intentosRestantes));
            PanelLateral.nivelDifi.setDisable(true);
            PanelLateral.ButtonInicio.setDisable(true);
            this.setDisable(false);//Activamos el gridPane para empezar la partida
            
            
        });
        
        //Timeline para la cuenta atras 
        timelineCuentaAtras = new Timeline(
            new KeyFrame(Duration.seconds(0.990), (ActionEvent t) -> {
            cuentaAtras--; 
            PanelLateral.textTiempo.setText(String.valueOf(cuentaAtras));
            textTiempo.setText(String.valueOf(cuentaAtras));
            if (cuentaAtras==0){
               timelineCuentaAtras.stop();
            }
                System.out.println(cuentaAtras);
                
               
        })
        );
        timelineCuentaAtras.setCycleCount(Timeline.INDEFINITE);
        //timelineCuentaAtras.play();
        //timelineCuentaAtras.play();

        //Timeline con espera de 4 segundos para ocultar las cartas
        timelineImagen = new Timeline(
            new KeyFrame(Duration.seconds(4.000), (ActionEvent t) -> {
               ocultarImagenes(); 
               mouseEvent(control); 
               //myButton.setCancelButton(false);   
        })
        );
        timelineImagen.setCycleCount(1);
        //timelineImagen.play();
      

    }
    /*
     * Asigna la imÃ¡gen que contendra cada casilla
    */
    public void anadeImagenes(){
        for(int x=0; x<Control.tamXTablero;x++){   
         for(int y=0; y<Control.tamYTablero;y++){
            carta = new Carta((byte)Control.tablero[x][y]);
            this.add(carta,x,y);

         }
       }  
    }
    
    /**
    * Asigna la posiciÃ³n de X e Y de las 2 cartas seleccionadas
    * @param control necesario para enviar parametros de cada click
    */
    public void mouseEvent(Control control){
        this.setOnMouseClicked((event) -> {
            //System.out.println("entra en mouse event");
            System.out.println("getx: "+event.getX());
            System.out.println("gety: "+event.getY());
            columnaTemporal = (byte)(event.getX() / Carta.TAM_CARTA);
            filaTemporal = (byte)(event.getY() / Carta.TAM_CARTA);
            System.out.println(columnaTemporal);
            System.out.println(filaTemporal);
            //if que controla que no se pueda volver a tapar una carta acertada.
            if(Control.encontrado[columnaTemporal][filaTemporal] == Control.SIPAREJA) {
                    imageOculta[columnaTemporal][filaTemporal].setVisible(false);
                    
            }else {//en el caso de que no este levantada levanta las correspondiente a las 2 deseadas.
                c++;
                System.out.println(c);
                if(c==1) { //Primer click
                    columna = columnaTemporal;
                    fila =  filaTemporal;
                    imageOculta[columna][fila].setVisible(false);

                    System.out.println("colum carta 1 " + columna);
                    System.out.println("fila carta 1 " + fila);
                    
                    if(urlAudio != null) {
                        try {
                            audioClip1 = new AudioClip(urlAudio.toURI().toString());
                            audioClip1.play();
                        } catch (URISyntaxException ex) {
                            System.out.println("Error en el formato de ruta de archivo de audio");
                        }            
                    } else {
                        System.out.println("No se ha encontrado el archivo de audio");
                    }
                    
                }else if (c==2){//Segundo click
                    columna1 = columnaTemporal;
                    fila1 = filaTemporal;
                    imageOculta[columna1][fila1].setVisible(false);

                    if(urlAudio != null) {
                        try {
                            audioClip1 = new AudioClip(urlAudio.toURI().toString());
                            audioClip1.play();
                        } catch (URISyntaxException ex) {
                            System.out.println("Error en el formato de ruta de archivo de audio");
                        }            
                    } else {
                        System.out.println("No se ha encontrado el archivo de audio");
                    }

                    System.out.println("colum carta 2 " + columna1);
                    System.out.println(" fila carta 2 " + fila1);
                    //System.out.println(c);
                    //If que controla si se pulsa dos veces sobre la misma carta no cuente el segundo click.
                    if(columna==columna1 && fila==fila1) {
                        c--;
                    }else {//Envia los cuatro parÃ¡metros de las 2 cartas seleccionadas.
                        intentosRestantes--;
                        textClick.setText(String.valueOf(intentosRestantes));
                        finPerdida();
                        control.buscarPareja(columna,fila,columna1,fila1);
                        controlAcierto = control.buscarPareja(columna,fila,columna1,fila1);
                        System.out.println(controlAcierto);
                        boolean fin = control.finPartida();
                        if(fin==true) {
                            finGanada();
                            
                        }
                        System.out.println("fin partida " +fin);
                    }
                    //System.out.println("colum carta1 " + columna);
                    //System.out.println("fila carta1 " + fila);
                    //System.out.println("colum carta2 " + columna1);
                    //System.out.println(" fila carta2 " + fila1);
                    
                    //Si la pareja mostrada es distinta llama al metodo para ocultarlas.
                    if (controlAcierto == false) {
                        ocultarImagenesDistintas();
                    }else {
                       c=0; 
                       
                    } 
                    //System.out.println(intentosRestantes);  
                }
            }
             
                
        });
        
    }
    
    /*
     * Oculta todas las imÃ¡genes del tablero.
    */
    private void ocultarImagenes() {
        for(int x=0; x<Control.tamXTablero;x++){   
            for(int y=0; y<Control.tamYTablero;y++){
                cartaBackground = new Image(getClass().getResourceAsStream("/images/30.jpg"));
                imgView = new ImageView(cartaBackground);
                this.add(imgView, x, y); 
                imageOculta[x][y]=imgView;

            }
        }  
    }
    
    /**
     * Oculta las imÃ¡genes cuando no coinciden
     */
    public void ocultarImagenesDistintas(){

        timelineocultaImagen = new Timeline(
            new KeyFrame(Duration.seconds(1.000), (ActionEvent t) -> {
                imageOculta[columna][fila].setVisible(true);//Oculta la imÃ¡gen levantada
                imageOculta[columna1][fila1].setVisible(true);//Oculta la imÃ¡gen levantada
                c=0;//Contador de clic en 0 una vez ocultada las imagenes para no mostrar mas de 2 imÃ¡genes 
 
        })
        );
        timelineocultaImagen.setCycleCount(1);
        timelineocultaImagen.play();
    }
    
    /**
     * Muestra un alert cuando los intentos llegan a 0
     */
    public void finPerdida(){
        
        if (intentosRestantes==0){
            System.out.println("entra en fin perdida");
            timelineEsperaFin = new Timeline(
            new KeyFrame(Duration.seconds(1.000), (ActionEvent t) -> {
               ocultarImagenes();//llama al metodo ocultarImagenes.
 
            })
            );
            timelineEsperaFin.setCycleCount(1);
            timelineEsperaFin.play();
            //System.out.println("fin partida perdida");
             
            AlertFin = new Alert(Alert.AlertType.INFORMATION);
            DialogPane dialogPane = AlertFin.getDialogPane();
            //dialogPane.setStyle("-fx-font: normal bold 15px 'serif'");
            AlertFin.getDialogPane().setGraphic(new ImageView("/images/fallo.PNG"));
            dialogPane.getStylesheets().add("css/myDialogs.css");
            dialogPane.setId("dialogo");
            dialogPane.getScene().setFill(Color.TRANSPARENT);
            AlertFin.setHeaderText(null);
            AlertFin.setTitle("Fin Partida");
            AlertFin.setContentText("Has perdido.");
            AlertFin.showAndWait();
             
            intentosRestantes=0;
            this.setDisable(true);//Desactivamos el gridPane para no poder realizar ninguna acciÃ³n
            //textClick.setVisible(false);
            reinicio();

        }
    }
    
    /**
     * Muestra un alert cuando consigue emparejar todas las cartas.
     */
    public void finGanada(){
        System.out.println("fin partida ganada");
        timelineEsperaFin = new Timeline(
            new KeyFrame(Duration.seconds(1.000), (ActionEvent t) -> {
               ocultarImagenes();
 
            })
        );
        timelineEsperaFin.setCycleCount(1);
        timelineEsperaFin.play();
        
        AlertFin = new Alert(Alert.AlertType.INFORMATION);
        DialogPane dialogPane = AlertFin.getDialogPane();
        //dialogPane.setStyle("-fx-font: normal bold 15px 'serif',-fx-border-radius: 0 0 18 18 ");
        AlertFin.getDialogPane().setGraphic(new ImageView("/images/superar.PNG"));
        dialogPane.getStylesheets().add("css/myDialogs.css");
        dialogPane.setId("dialogo");
        AlertFin.setHeaderText(null);
        AlertFin.setTitle("Fin Partida");
        AlertFin.setContentText("Has Ganado.");
        AlertFin.showAndWait();
        intentosRestantes=0;
        this.setDisable(true);//Desactivamos el gridPane para no poder realizar ninguna acciÃ³n
        //textClick.setVisible(false);
        reinicio();
        
    }
    
    /**
     * llama a la clase control para crear un nuevo array y reiniciar todas las variables necesarias.
     */
    public void reinicio(){
        control= new Control();
        //textClick.setVisible(true);
        //System.out.println("nivel de dificultad " + PanelLateral.continuidad);
        intentosRestantes=15;
        cuentaAtras = 4;
        ocultarImagenes();
        muestraImagenesInicio(control);
        PanelLateral.nivelDifi.setDisable(false);
        PanelLateral.nivelDifi.getSelectionModel().select("Medio");
        PanelLateral.ButtonInicio.setDisable(false);
       
        
        
    }
    
    public void eliminarCartas(){
        
        for(int x=0; x<=23;x++){   
            this.getChildren().remove(x);
        }  
        //this.getChildren().size();

        System.out.println(this.getChildren().size());

    }

}
```

## Código Clase PanelLateral
```
package es.adriansoriagarcia.proyectofinaljavafxii;

import javafx.geometry.Pos;
import javafx.scene.control.Button;
import javafx.scene.control.ChoiceBox;
import javafx.scene.control.Label;
import javafx.scene.image.ImageView;
import javafx.scene.layout.VBox;
import javafx.scene.paint.Color;
import javafx.scene.text.Font;
import javafx.scene.text.Text;

/**
 *
 * @author usuario
 */
public class PanelLateral extends VBox{
    
    static Label labelCuentaAtras;//Declaración de label para la cuenta atras.
    static Text textTiempo;//Declaración de text que contiene el tiempo restante.
    static Text texTitleTempo;//Declaración de text que contiene el texto "tiempo".
    static Text textClick;//Declaración de text que contiene los click restantes.
    static Text texClickRestante;//Declaración de text que contiene el texto "intentos".
    final int TEXT_SIZE = 24;//Declaración e inicialización de usada para el tamaño de letra de los text. 
    static int selectedNivel;//Indice del nivel de dificultad.
    static ChoiceBox<String> nivelDifi;//Declaración de choicebox con los diferentes niveles de dificultad.
    static Button ButtonInicio;//Declaración botón de incio partida.
    static Button btnStart;//Declaración botón de activar sonido.
    static Button btnPause;//Declaración botón de desactivar sonido.
    
    
    public PanelLateral(){
        this.setAlignment(Pos.CENTER);//Posición centrada del panel.
        //----------------------------------------------------------------------
            //BOTONES SONIDO
        //----------------------------------------------------------------------
        
        btnStart.setId("soundON");
        btnPause.setId("soundOFF");
        btnStart.setGraphic(new ImageView("/images/sound.PNG"));
        btnPause.setGraphic(new ImageView("/images/mute.PNG"));
        this.getChildren().addAll(btnStart,btnPause);
        //----------------------------------------------------------------------
        nivelDifi = new ChoiceBox<>();
        nivelDificultad();
        layoutPanel();
        //----------------------------------------------------------------------
            //BOTON INICION
        //----------------------------------------------------------------------
        ButtonInicio.setMaxSize(100,50);
        this.getChildren().add(ButtonInicio);
        //this.getStylesheets().add("/stylesheet.css");
        this.setId("panel");
        //this.setStyle("-fx-font: normal bold 15px 'serif' ");
        //----------------------------------------------------------------------

    }
    
    private void nivelDificultad(){
        //----------------------------------------------------------------------
            //CHOICEBOX OPCIONES
        //----------------------------------------------------------------------
        
        nivelDifi.getSelectionModel().select("Medio");

        nivelDifi.getItems().add("Facil");
        nivelDifi.getItems().add("Medio");
        nivelDifi.getItems().add("Dificil");

        /*nivelDifi.getSelectionModel()
            .selectedItemProperty()
            .addListener( (ObservableValue<? extends String> observable, String oldValue, String newValue) -> System.out.println(newValue) );*/
        
        nivelDifi.setOnAction((evento) -> {
        selectedNivel = nivelDifi.getSelectionModel().getSelectedIndex();
        String  selectedItem  = nivelDifi.getSelectionModel().getSelectedItem();
        //System.out.println("Selección realizada: [" + selectedNivel + "] " +selectedItem );
            switch (selectedNivel) {
                case 0:
                    System.out.println("nivel facil");
                    Tablero.intentosRestantes=20;
                    textClick.setText(String.valueOf(Tablero.intentosRestantes));
                    break;
                case 1:
                    System.out.println("nivel medio");
                    Tablero.intentosRestantes=15;
                    textClick.setText(String.valueOf(Tablero.intentosRestantes));
                    break;
                case 2:
                    System.out.println("nivel dificil");
                    Tablero.intentosRestantes=10;
                    textClick.setText(String.valueOf(Tablero.intentosRestantes));
                    break;
                default:
                    break;
            }
        });
        
        this.getChildren().add(nivelDifi);
        
    }
    
    private void layoutPanel(){
        this.setSpacing(10);//Espacio entre componentes.
        //Texto de etiqueta para tiempo
        texTitleTempo = new Text("Tiempo:");
        texTitleTempo.setFont(Font.font(TEXT_SIZE));
        texTitleTempo.setFill(Color.BLACK);
        //Texto para el tiempo restante
        textTiempo = new Text("4");
        textTiempo.setFont(Font.font(TEXT_SIZE));
        textTiempo.setFill(Color.BLACK);
        this.getChildren().add(texTitleTempo);
        this.getChildren().add(textTiempo);

        //-------------------------------------------------
        //Texto de etiqueta para tiempo
        texClickRestante = new Text("Intentos:");
        texClickRestante.setFont(Font.font(TEXT_SIZE));
        texClickRestante.setFill(Color.BLACK);
        //Texto para intentos restante
        textClick = new Text("15");
        textClick.setFont(Font.font(TEXT_SIZE));
        textClick.setFill(Color.BLACK);
        this.getChildren().add(texClickRestante);
        this.getChildren().add(textClick);
          
    }
    
}
```



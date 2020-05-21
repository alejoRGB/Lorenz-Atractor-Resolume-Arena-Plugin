# Lorenz-Atractor-Resolume-Arena-Plugin
This is the repository of a Resolume Arena plugin using Processing and the Lorenz Atractor
Este es el repositorio para un Plugin de Resolume Arena usando el Atracktor de Lorenz, hecho con Processing. 


<img src = "fotos/2020-05-21%20at%2019.32.31.png" width =300 height = 300><img src = "fotos/2020-05-21%20at%2019.33.21.png" width =300 height = 300><img src = "fotos/2020-05-21%20at%2019.33.28.png" width =300 height = 300>

<img src = "fotos/2020-05-21%20at%2019.33.39.png" width =300 height = 300><img src = "fotos/2020-05-21%20at%2019.33.49.png" width =300 height = 300>


//Lorenz Atracktor

//Este plugin lo que hace es dibujar puntos siguiendo el sistema de coordanadas de la ecuación "El Atracktor de Lorenz" , que en ingles significa
//"Loren Atracktor"*(1). Luego una camara se mueve aleatoriamente a lo largo de 10 puntos a una detereminada distacia y captura la animación.
//Para correr este plugin es necesario tener instalado Spout controls, y tener las librerias de processing utilizadas en la misma carpeta que
//el plugin. Las librerias vienen en el paquete de Github. Para descargar Spout Controls dejo un link abajo. 
//Gracias a https://processing.org/, https://discourse.processing.org/, a https://hamoid.com/ o @hamoid en el foro de processing. 
//Esta app se descarga gratuitamente del link: 


//Fuentes:
//1: https://es.wikipedia.org/wiki/Atractor_de_Lorenz
//2: Foro de The Processing Foundation: https://discourse.processing.org/t/peasycam-automatic-camera-movements/20515
//3: El video de The coding train: https://www.youtube.com/watch?v=f0lkz2gSsIk&t=1116s
//4: Pagina web de the Coding Challenge: https://thecodingtrain.com/CodingChallenges/012-lorenzattractor.html
//5: 

//Como Instalar este plugin en Resolume Arena 6 en Windows 10:

//  1. Van a C:\Program Files\Resolume Arena 6\plugins\vfx
//  2. Pegan la carpeta "Lorenz_Atracktor" ahi.
//  3. Cuando abran Resolume debería aparecer el 
//     plugin en la pestaña de FUENTES o SOURCES




import peasy.PeasyCam;
import peasy.org.apache.commons.math.geometry.*;
import peasy.*;
import spout.*; 

PeasyCam cam;
Spout spout;
String sendername; //nombre del plugin
//Extensiones de SPOUT

//CONTROL ARRAYS
String[] controlName;
int[] controlType;
float[] controlValue;
String[] controlText;

// CONTROL VARIABLES
boolean linoPun = true;
boolean pointsOrLine = true;
float largoDeLineas;

float cantLin=10;
float velcam;

float RotationSpeed = 1.0;
float RotX = 0;
float RotY = 0;
String UserText = "";






//Initializing the cameraStates
Rotation rot1 = new Rotation(RotationOrder.XYZ, radians(0), radians(0), radians(0));
Vector3D center1 = Vector3D.zero;
double distancee1 = 450; // from this far away

Rotation rot2 = new Rotation(RotationOrder.XYZ, radians(90), radians(36), radians(0));
Vector3D center2 = Vector3D.zero;
double distancee2 = 500; // from this far away

Rotation rot3 = new Rotation(RotationOrder.XYZ, radians(190), radians(72  ), radians(0));
Vector3D center3 = Vector3D.zero;
double distancee3 =500; // from this far away

Rotation rot4 = new Rotation(RotationOrder.XYZ, radians(270), radians(108 ), radians(0));
Vector3D center4 = Vector3D.zero;
double distancee4 = 500; // from this far away

Rotation rot5 = new Rotation(RotationOrder.XYZ, radians(0), radians(144), radians(85));
Vector3D center5 = Vector3D.zero;
double distancee5 = 500; // from this far away

Rotation rot6 = new Rotation(RotationOrder.XYZ, radians(90), radians(180), radians(95));
Vector3D center6 = Vector3D.zero;
double distancee6 = 500; // from this far away

Rotation rot7 = new Rotation(RotationOrder.XYZ, radians(190), radians(216), radians(75));
Vector3D center7 = Vector3D.zero;
double distancee7 = 500; // from this far away

Rotation rot8 = new Rotation(RotationOrder.XYZ, radians(270), radians(252), radians(105));
Vector3D center8 = Vector3D.zero;
double distancee8= 500; // from this far away

Rotation rot9 = new Rotation(RotationOrder.XYZ, radians(270), radians(288), radians(105));
Vector3D center9 = Vector3D.zero;
double distancee9= 500; // from this far away

Rotation rot10 = new Rotation(RotationOrder.XYZ, radians(270), radians(324), radians(105));
Vector3D center10 = Vector3D.zero;
double distancee10= 500; // from this far away

CameraState state1 = new CameraState(rot1, center1, distancee1);
CameraState state2 = new CameraState(rot2, center2, distancee1);
CameraState state3 = new CameraState(rot3, center3, distancee1);
CameraState state4 = new CameraState(rot4, center4, distancee1);
CameraState state5 = new CameraState(rot5, center5, distancee1);
CameraState state6 = new CameraState(rot6, center6, distancee1);
CameraState state7 = new CameraState(rot7, center7, distancee1);
CameraState state8 = new CameraState(rot8, center8, distancee1);
CameraState state9 = new CameraState(rot9, center9, distancee1);
CameraState state10 = new CameraState(rot10, center10, distancee1);
int r ;
ArrayList<PVector> points = new ArrayList<PVector>();

float x =0.01;
float y =0;
float z =0;

float dx;
float dy;
float dz;

float a = 10; 
float b = 28;
float c = 8.0/3.0;

float dt = 0.01;
float hu =0;

boolean coloreado = true;
float h;


long nextEvent = 0;

void setup() {
  spout = new Spout(this);
  sendername = "Lorenz_Atractor";
  spout.createSender(sendername, width, height);
  size (1000, 1000, P3D);




  controlName = new String[20];
  controlType = new int[20];
  controlValue = new float[20];
  controlText = new String[20];


  //Control lineas o puntos:
  spout.createSpoutControl("Lineas o Puntos", "bool", 1); 
  linoPun = true;

  spout.createSpoutControl("Largo de Lineas", "float", 0, 127.00, 60.00);
  spout.createSpoutControl("Cant Lineas", "float", 0, 127.0, 60.0);
  spout.createSpoutControl("Vel Cam", "float", 0, 127.0, 60.0);

  UserText = "";
  spout.openSpoutControls(sendername);

  colorMode(HSB);
  cam = new PeasyCam(this, width/2, height/2, 0, 400);

  cam.setMinimumDistance(10);
  //Posición random inicial para la camara
  r= int(random(0, 3));

  spout.openSpoutControls(sendername);
}

void draw() {

  background(0);
  pushMatrix();

  scale(5);

  // CHECK FOR UPDATED CONTROLS FROM THE CONTROLLER
  int nControls = spout.checkSpoutControls(controlName, controlType, controlValue, controlText);
  if (nControls > 0) {
    // For all controls
    for (int i = 0; i < nControls; i++) {
      // Check each control by name

      // "Lineas o Puntos"
      if (controlName[i].equals("Lineas o Puntos")) {
        linoPun = (boolean)(controlValue[i] == 1);
      }
      if (controlName[i].equals("Largo de Lineas")) {
        float newNum = controlValue[i];
        //Setea cuan largas son las lineas que se dibujan
        largoDeLineas = map (newNum, 0, 127, 0, 1000);
      }
      if (controlName[i].equals("Cant Lineas")) {
        float cantt = controlValue[i];

        cantLin = cantt;
      }
       if (controlName[i].equals("Vel Cam")) {
        float vel = controlValue[i];
        velcam = vel;
      }
    }
  }






  dx = (a * (y - x))*dt; 
  x = x+dx;

  dy = ((x *(b-z))-y)*dt;
  y = y+dy;

  dz = (x*y- c*z)*dt;
  z = dz+z;

  //Setea dibujar lineas o puntos
  //true hace puntos, false hace una linea

  pointsOrLine = linoPun;

  //Indica si las lineas son monocromaticas(true) o tienen una gama de colores
  //Por default esta en gama de color;true = gama de color  false = monocromatico
  //este variable al final no se puede modificar desde el resolume, usar 
  //el efecto colorize para cambiar el color

  coloreado = true ;
  if ( coloreado) {
    h = 0.01;
  } else {
    h = 0;
    hu = 0;
  }
  strokeWeight(1);
  noFill();
  points.add(new PVector(x, y, z));

  hu =0;

  // El valor que se le pasa a la funcion indica cuantas lineas 
  //se van a dibujar

  cuantosDibujo(cantLin);

  //Velocidad con la que se mueve la camara entre los puntos
  int velCam =int(map(velcam,0,127,100,7000));

  if (millis() >= nextEvent) {
    
    r += int(random(1, 8));
    // Si nos pasamos del máx, retrocede una "página"
    if (r >= 10) {
      r = 0;
    }
    if (r == 0) {
      cam.setState(state1, velCam);
    } else if (r == 1) {
      cam.setState(state2, velCam);
    } else if (r == 2) {
      cam.setState(state3, velCam);
    } else if (r == 3) {
      cam.setState(state4, velCam);
    } else if (r == 4) {
      cam.setState(state5, velCam);
    } else if (r == 5) {
      cam.setState(state6, velCam);
    } else if (r == 6) {
      cam.setState(state7, velCam);
    } else if (r == 7) {
      cam.setState(state8, velCam);
    } else if (r == 8) {
      cam.setState(state9, velCam);
    } else if (r == 9) {
      cam.setState(state10, velCam);
    }

    nextEvent = millis() + velCam;
    //println(cantLin);
  }

  //box(10);

  popMatrix();
  spout.sendTexture();
}


void cuantosDibujo(float a) {
  for (int i = 0; i <a; i++) {
    dibujar(i/3);
    dibujar(-i/3);
  }
}
void dibujar(float mm) {

  if (pointsOrLine == false) {
    beginShape();

    for (int i =0; i<points.size()-1; i++) {
      PVector p = points.get(i);
      stroke(hu, 255, 255);
      vertex(p.x*mm, p.y, p.z); 

      hu+=h;
      if (hu>255) {
        hu = 0;
      }
      if (points.size()>largoDeLineas) {
        points.remove(0);
      }
    }

    endShape();
  } else if (pointsOrLine == true) {
    for (int i =0; i<points.size()-1; i++) {
      PVector p = points.get(i);
      stroke(hu, 255, 255);
      point(p.x*mm, p.y, p.z); 

      hu+=h;
      if (hu>255) {
        hu = 0;
      }
      if (points.size()>largoDeLineas) {
        points.remove(0);
      }
    }
  }
}



void exit() {
  spout.closeSpoutControls();
  super.exit();
}

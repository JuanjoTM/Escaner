import processing.serial.*;

Serial port;
PrintWriter output;
int valor = 0;
int filas = 1;
int columnas = 0;
int pos = 1;

//Variables de los botones
//BOTON DE COMIENZO
int xCom = 10;
int yCom = 10;
int dyCom = 70;
int dxCom = 180;

//BOTON DE REINICIO
int yRen = 90;
int xRen = 10;
int dyRen = 70;
int dxRen = 180;

void setup(){
  port = new Serial(this, Serial.list()[0], 9600);
  output = createWriter("texto.txt");
  size(200, 300);
}

void draw(){
  background(255,255,255);
  if(mouseX < xCom+dxCom && mouseX > xCom && mouseY > yCom && mouseY < yCom+dyCom){
    fill(0,255,0);
  }
  else{
    fill(255,0,0);
  }
  rect(xCom, yCom, dxCom, dyCom);
  
  if(mouseX < xRen+dxRen && mouseX > xRen && mouseY > yRen && mouseY < yRen+dyRen){
    fill(0,255,0);
  }
  else{
    fill(255,0,0);
  }
  rect(xRen, yRen, dxRen, dyRen);
  
  PImage imagen = loadImage("escaner.jpg");
  image(imagen, 10, 180, 180, 115);
  
  fill(0,0,0);
  text("Iniciar", 85, 50);
  text("Reset", 90, 130);
  
  //output.println("CREATE (Punto"+pos+":Fila"+filas+" {Pixel: \""+valor+"\"})");
  if(port.read()==1 && filas<15){
    if(columnas>=15){
      columnas = 0;
      filas += 1;
    }
    valor = port.read();
    output.println("CREATE (Punto"+pos+":Fila"+filas+" {Pixel: \""+valor+"\"})");
    pos += 1;
    columnas += 1;
  }
}

void mousePressed(){
 if(mouseX < xCom+dxCom && mouseX > xCom && mouseY > yCom && mouseY < yCom+dyCom){
    port.write(1);
  }
 else if(mouseX < xRen+dxRen && mouseX > xRen && mouseY > yRen && mouseY < yRen+dyRen){
    port.write(2);
  } 
}

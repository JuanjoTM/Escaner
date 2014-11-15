
//Variables de uso general
int distancia = 0; // Record the number of steps we've taken 
int pinDir = 8;
int paso = 250; //Controla el area de lectura del fotoresistor (pixeles)
int direccion = 1;
int pinDirY = 11;
int imprimir = 0;
int distanciaY = 0;

//Variables Fotoresistor
const int pinFoto = A2;
int linea[225]; //Pixeles por linea
int pos = 0;

//Variables Eje X
int contadorX = 0;
int pinX = 9;

//Variables Eje Y
int contadorY = 0;
int pinY = 10;

//Dimensiones del objeto
int altura = 0;
int anchura = 0;

int lectura = 0;

int comando = 0;
int reset = 0;
void setup() {
  //Modos de los pines
  pinMode(pinDir, OUTPUT);
  pinMode(pinX, OUTPUT);
  pinMode(pinY, OUTPUT);
  pinMode(pinFoto, INPUT);
  
  //Valores iniciales de la direccion y los ejes
  digitalWrite(pinDirY, HIGH);
  digitalWrite(pinDir, HIGH);
  digitalWrite(pinX, LOW);
  digitalWrite(pinY, LOW);
  
  Serial.begin(9600);
}

void loop() {
  //Lectura del fotoresistor
  lectura = analogRead(pinFoto);
  comando = Serial.read();
  
  //Guardando el pixel
  if(lectura<450){
    linea[pos] = 1;
  }
  else{
    linea[pos] = 0;
  }
  
  //Señal cuadrada para el eje x
  while(comando==1){
    if (contadorX <= 15){
      digitalWrite(pinX, HIGH);
      delayMicroseconds(100);
      digitalWrite(pinX, LOW);
      delayMicroseconds(100);
      
      if(distancia == paso){
        //Reseteo para un nuevo pixel hasta completar 10
        distancia = 0;
        contadorX = contadorX + 1;
        pos = pos + 1;
        //Tiempo de espera
        delay(150);
      }
    }
    //Señal cuadrada para el eje y
    else if (contadorY<=15 && pos<=225){
      digitalWrite(pinY, HIGH);
      delayMicroseconds(100);
      digitalWrite(pinY, LOW);
      delayMicroseconds(100);
      
      if(distancia == paso){
        
        //Los valores se modifican para una nueva linea
        distancia = 0;
        contadorX = 0;
        contadorY = contadorY + 1;
        pos = pos + 1;
        delay(150);
        //La direccion de cambia
        if (direccion == 1){
          
          direccion = 0;
          digitalWrite(pinDir, LOW);
        }
        else{
          
          direccion = 1;
          digitalWrite(pinDir, HIGH);
        }
      }
    }
    //contador de distancia avanzada del eje x
    distancia = distancia + 1;
    if(contadorY == 15){
      /*if(imprimir == 0){
        int cant = 0;
        for(int i = 0; i<225; i++){
          Serial.print(linea[i]);
          cant = cant + 1;
          if (cant == 15){
            Serial.println();
            cant = 0;
          }
        }
      }*/
      imprimir = 1;
      comando = 0;
      
    }
  }
  
  while(comando==2){
    if(reset==0){
      contadorX = 0;
      contadorY = 0;
      direccion = 1;
      pos = 0;
      imprimir = 0;
      distancia = 0;
      distanciaY = 0;
      digitalWrite(pinDir, LOW);
      digitalWrite(pinDirY, LOW);
    }
    
    if(distancia <= 3750){
      digitalWrite(pinX, HIGH);
      delayMicroseconds(100);
      digitalWrite(pinX, LOW);
      delayMicroseconds(100);
      distancia += 1;
      reset = 1;
      //Tiempo de espera
      //delay(150);
    }else if(distanciaY<=3750){
      digitalWrite(pinY, HIGH);
      delayMicroseconds(100);
      digitalWrite(pinY, LOW);
      delayMicroseconds(100);
      distanciaY += 1;
      
    }
    else{
      comando = 0;
      reset = 0;
    }
    
  }
  
}

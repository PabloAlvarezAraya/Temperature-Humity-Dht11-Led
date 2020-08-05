/*
          #############%@    
          ####   #########%  
          ##       #########&
          #%       ##########
          ###    ############
          ###################     
          ###################    
           ##################
            #################
               ##############
                           ##
                            #
*/  
//Modulo sensor de Temperatura y Humedad Dht11
// enciende luz con parámetros de temperatura y humedad



#include <DHT.h> // Incluimos librería
 

#define DHTPIN 2 // Definimos el pin digital donde se conecta el sensor
#define DHTTYPE DHT11 // Dependiendo del tipo de sensor
int LedPin = 13;

DHT dht(DHTPIN, DHTTYPE); // Inicializamos el sensor DHT11
 
void setup() {
  
  Serial.begin(9600); // Inicializamos comunicación serie
 
 
  dht.begin();  // Comenzamos el sensor DHT

  pinMode (LedPin,OUTPUT) ;
 
}
 
void loop() {
    // Esperamos 5 segundos entre medidas
  delay(5000);
 
  // Leemos la humedad relativa
  float h = dht.readHumidity();
  // Leemos la temperatura en grados centígrados (por defecto)
  float t = dht.readTemperature();
  

 
  // Comprobamos si ha habido algún error en la lectura
  if (isnan(h) || isnan(t)) {
    Serial.println("Error obteniendo los datos del sensor DHT11");
    return;
  }
 
 
  // Calcular el índice de calor en grados centígrados
  float hic = dht.computeHeatIndex(t, h, false);
 
  Serial.print("Humedad: ");
  Serial.print(h);
  Serial.print(" %\t");
  Serial.print("Temperatura: ");
  Serial.print(t);
  Serial.print(" *C ");
  Serial.print("Sensacion Termica: ");
  Serial.print(hic);
  Serial.println(" *C ");

  if ( ( h > 33 ) || ( t > 26 ) ) { // si la humeldad (h) es mayor a 31ª o la temperatura (t) es mayor a 22ª 
    digitalWrite (LedPin, HIGH); // se prende el led del arduino
   }
    else {
    digitalWrite (LedPin, LOW);  } // si no, se apaga

}

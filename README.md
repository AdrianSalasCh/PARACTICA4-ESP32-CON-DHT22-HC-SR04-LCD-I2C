# PARACTICA4-ESP32-CON-DHT22-HC-SR04-LCD-I2C
En esta practica se hará la conexión de un ESP32 con un HC-SR04 y un DHT22, mostrando la información en un LCD I2C

## INTRODUCCIÓN

### DESCRIPCIÓN
La ESP32 la utilizamos en un entorno de adquision de datos, lo cual en esta practica ocuparemos un sensor ultrasónico (HC-SR04) para adquirir la distancia y un DHT para la temperatura y la humedad; cabe aclarar que esta practica se usara un simulador llamado WOKWI.

### MATERIAL NECESARIO

Para realizar esta practica necesitas lo siguiente
- [WOKWI](https://wokwi.com/)
- Tarjeta ESP 32
- Sensor HC-SR0 Ultrasonic Distance Sensor
- DHT22
- LCD 16x2 (I2C)

### INSTRUCCIONES

**REQUISITOS PREVIOS**
Para poder usar este repositorio necesitas entrar a la plataforma [WOKWI](https://wokwi.com/)

### Instrucciones de preparación de entorno

1. Abrir la terminal de programación y colocar la siguente programación:

```
#include "DHTesp.h"
#include <LiquidCrystal_I2C.h>
#define I2C_ADDR    0x27
#define LCD_COLUMNS 20
#define LCD_LINES   4

LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

const int DHT_PIN = 13;
const int Trigger = 4;   //Pin digital 2 para el Trigger del sensor
const int Echo = 15;   //Pin digital 3 para el Echo del sensor

DHTesp dhtSensor;

void setup() {

  Serial.begin(9600);//iniciailzamos la comunicación
  pinMode(Trigger, OUTPUT); //pin como salida
  pinMode(Echo, INPUT);  //pin como entrada
  digitalWrite(Trigger, LOW);//Inicializamos el pin con 0

  dhtSensor.setup(DHT_PIN, DHTesp::DHT22);

  lcd.init();
  lcd.backlight();
}

void loop()
{

  long t; //timepo que demora en llegar el eco
  long d; //distancia en centimetros

  digitalWrite(Trigger, HIGH);
  delayMicroseconds(10);          //Enviamos un pulso de 10us
  digitalWrite(Trigger, LOW);
  
  t = pulseIn(Echo, HIGH); //obtenemos el ancho del pulso
  d = t/59;             //escalamos el tiempo a una distancia en cm
  
  TempAndHumidity  data = dhtSensor.getTempAndHumidity();
  Serial.println("Temp: " + String(data.temperature, 1) + "°C");
  Serial.println("Humidity: " + String(data.humidity, 1) + "%");
  Serial.println("---");

  Serial.print("Distancia: ");
  Serial.print(d);      //Enviamos serialmente el valor de la distancia
  Serial.print("cm");
  Serial.println();
  delay(1000);          //Hacemos una pausa de 100ms
  
  lcd.setCursor(0, 0);
  lcd.print("Diplomado");
  lcd.setCursor(0, 1);
  lcd.print("Automatizacion");
  delay (2000);
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Adrian Salas");
  lcd.setCursor(0, 1);
  lcd.print("lng Industrial");
  delay (2000);
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print(" 07/06/2025");
  delay(2000);
  lcd.clear();

  lcd.setCursor(0, 0);
  lcd.print("Distancia: "+ String (d)+"cm");
  delay (2000);
  lcd.clear();

  lcd.setCursor(0, 0);
  lcd.print("  Temp: " + String(data.temperature, 1) + "\xDF"+"C  ");
  lcd.setCursor(0, 1);
  lcd.print(" Humidity: " + String(data.humidity, 1) + "% ");
  delay(2000);
  lcd.clear();
}
```

2. Instalar las siguientes librerias:
      - **LiquidCrystal I2C**
      - **DHT sensor library for ESPx**
   Como se muestra en las siguentes imagenes.

![](https://github.com/AdrianSalasCh/PARACTICA4-ESP32-CON-DHT22-HC-SR04-LCD-I2C/blob/main/LiquidCrystal%20I2C%20P4.PNG)

![](https://github.com/AdrianSalasCh/PARACTICA4-ESP32-CON-DHT22-HC-SR04-LCD-I2C/blob/main/DHT%20sensor%20library%20for%20ESPx%20P4.PNG)

3. Hacer la conexion del HC-SR0 y el DHT22 con la ESP32 y el LCD 16x2 (I2C) como se muestra en la siguente imagen.

![](https://github.com/AdrianSalasCh/PARACTICA4-ESP32-CON-DHT22-HC-SR04-LCD-I2C/blob/main/CONEXION%20P4.PNG)

### Instrucciónes de operación

1. Iniciar simulador.
2. Visualizar los datos en el monitor serial y en el LCD.
3. Colocar la distancia dando *doble click* al sensor **HC-SR0**
4. Colocar la humedad y la temperatura dando *doble click* al sensor **DHT22**

## RESULTADOS

Cuando haya funcionado, verás los valores dentro del monitor serial y en el LCD como se muestra en las siguentes imagenes.

![](https://github.com/AdrianSalasCh/PARACTICA4-ESP32-CON-DHT22-HC-SR04-LCD-I2C/blob/main/SIMULACI%C3%93N%20TERMINADA%20P4-1.PNG)

![](https://github.com/AdrianSalasCh/PARACTICA4-ESP32-CON-DHT22-HC-SR04-LCD-I2C/blob/main/SIMULACI%C3%93N%20TERMINADA%20P4-2.PNG)

## CRÉDITOS

Desarrollado por Ing. Luis Adrián Salas Chávez
- [GitHub](https://github.com/)

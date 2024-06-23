# PRACTICA 5: Buses de Comunicación I (Introducción y I2C)

## Descripción
El objetivo de esta práctica es comprender el funcionamiento de los buses de comunicación entre periféricos, que pueden ser internos o externos al procesador. Esta es la primera práctica de una serie de cuatro donde se estudiarán los buses I2C, SPI, I2S y USART. En cada caso, realizaremos una práctica donde controlaremos un periférico de ejemplo.

## Introducción Teórica

### Función del Bus
El bus permite la conexión lógica entre los subsistemas del computador, transmitiendo señales eléctricas a través de conductores metálicos con la ayuda de circuitos integrados que manejan un protocolo para transmitir datos útiles, direcciones y señales de control.

### Tipos de Buses
1. **Bus Paralelo**: Envía datos por bytes simultáneamente a través de varias líneas dedicadas.
2. **Bus Serie**: Envía datos bit a bit y los reconstruye mediante registros o rutinas. Ejemplos incluyen USB, SATA, RS232, SPI, I2C e I2S.

## Bus I2C
El estándar I2C, desarrollado por Philips en 1982, requiere únicamente dos cables: uno para la señal de reloj (CLK) y otro para el envío de datos (SDA). Cada dispositivo tiene una dirección única, y la arquitectura es de tipo maestro-esclavo, donde el maestro inicia la comunicación.

### Funcionamiento del Bus I2C
La comunicación se realiza mediante tramas que incluyen:
- 7 bits para la dirección del dispositivo esclavo.
- 1 bit para indicar si se desea enviar o recibir información.
- 1 bit de validación.
- Datos enviados o recibidos del esclavo.
- Otro bit de validación.

## Ventajas y Desventajas del Bus I2C

### Ventajas
- Requiere pocos cables.
- Mecanismos para verificar la llegada de la señal.

### Desventajas
- Velocidad media-baja.
- No es full duplex (es half duplex).
- No hay verificación de que el contenido del mensaje es correcto (solo de la llegada del mensaje).

## Ejercicio Práctico 1: Escáner I2C
Programar el siguiente código y colocar varios dispositivos I2C.

```cpp
#include <Arduino.h> 
#include <Wire.h> 

void setup() { 
  Wire.begin(); 
  Serial.begin(115200); 
  while (!Serial);             // Leonardo: wait for serial monitor 
  Serial.println("\nI2C Scanner"); 
} 

void loop() { 
  byte error, address; 
  int nDevices; 
  
  Serial.println("Scanning..."); 
  nDevices = 0; 
  for(address = 1; address < 127; address++ ) { 
    Wire.beginTransmission(address); 
    error = Wire.endT

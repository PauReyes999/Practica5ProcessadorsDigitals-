# Práctica 5: Buses de Comunicación I (Introducción y I2C)

## Objetivo
El objetivo de esta práctica es comprender el funcionamiento de los buses de comunicación entre periféricos, específicamente el bus I2C. Aprenderemos a controlar un periférico de ejemplo: un display OLED SSD1306.

## Introducción Teórica

### Buses de Comunicación
- **Bus Paralelo**: Envía datos por bytes simultáneamente a través de varias líneas.
- **Bus Serie**: Envía datos bit a bit por pocos conductores. Ejemplos: USB, SATA, SPI, I2C.

### Bus I2C
Desarrollado por Philips en 1982, el bus I2C (Inter-Integrated Circuit) permite la comunicación entre dispositivos usando solo dos cables: uno para la señal de reloj (SCL) y otro para los datos (SDA).

#### Características:
- **Dirección única para cada dispositivo**: 7 bits de dirección.
- **Arquitectura maestro-esclavo**: El maestro inicia la comunicación.
- **Resistencias de Pull-UP**: Necesarias para mantener las líneas a Vcc.

## Conexiones

### ESP32 y OLED SSD1306

| I2C Device | ESP32                   |
|------------|-------------------------|
| SDA        | GPIO 21 (default)       |
| SCL        | GPIO 22 (default)       |
| GND        | GND                     |
| VCC        | 3.3V o 5V               |

### Ejemplo de Código: Escáner I2C

```cpp
#include <Arduino.h>
#include <Wire.h>

void setup() {
  Wire.begin();
  Serial.begin(115200);
  while (!Serial);
  Serial.println("\nI2C Scanner");
}

void loop() {
  byte error, address;
  int nDevices;

  Serial.println("Scanning...");
  nDevices = 0;
  for (address = 1; address < 127; address++) {
    Wire.beginTransmission(address);
    error = Wire.endTransmission();

    if (error == 0) {
      Serial.print("I2C device found at address 0x");
      if (address < 16) Serial.print("0");
      Serial.println(address, HEX);
      nDevices++;
    } else if (error == 4) {
      Serial.print("Unknown error at address 0x");
      if (address < 16) Serial.print("0");
      Serial.println(address, HEX);
    }
  }
  if (nDevices == 0) Serial.println("No I2C devices found\n");
  else Serial.println("done\n");
  delay(5000);
}

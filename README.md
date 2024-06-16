# Practica 5
# Part A

Codi en Línea

#include <Arduino.h>
#include <Wire.h>
void setup()
{
  Wire.begin(8, 9);
  Serial.begin(115200);
  while (!Serial); // Leonardo: wait for serial monitor
    Serial.println("\nI2C Scanner");
}
void loop()
{
  byte error, address;
  int nDevices;
  Serial.println("Scanning...");
  nDevices = 0;
  for(address = 1; address < 127; address++ )
  {
  // The i2c_scanner uses the return value of
  // the Write.endTransmisstion to see if
  // a device did acknowledge to the address.
    Wire.beginTransmission(address);
    error = Wire.endTransmission();
    Serial.println (address);
    if (error == 0)
    {
      Serial.print("I2C device found at address 0x");
      if (address<16)
        Serial.print("0");
      Serial.print(address,HEX);
      Serial.println(" !");
      nDevices++;
    }
    else if (error==4)
    {
      Serial.print("Unknown error at address 0x");
      if (address<16)
        Serial.print("0");
      Serial.println(address,HEX);
    }
  }
  if (nDevices == 0)
    Serial.println("No I2C devices found\n");
  else
    Serial.println("done\n");
  delay(5000); // wait 5 seconds for next scan
}
Explicació del codi

## 1.Inclusió de llibreries

#include <Arduino.h>
#include <Wire.h>
Aquí s'inclouen les llibreries per utilitzar les funcions bàsiques d'Arduino i comunicació I2C.

## 2.Setup

void setup() {
  Wire.begin(8, 9);
  Serial.begin(115200);
  while (!Serial); // Leonardo: wait for serial monitor
  Serial.println("\nI2C Scanner");
}
'Wire.begin(8, 9);:' Aquí s'inicialitza la comunicació I2C al ESP32 utilitzant els pins GPIO 8 (SDA) i 9 (SCL).
'Serial.begin(115200);:' Aquí s'inicia la comunicació serial a 115200 bauds.
'while (!Serial);:' Aquí espera fins que el port serial estigui llest.
'Serial.println("\nI2C Scanner");:' Mostra per pantalla el missatge "I2C Scanner" al monitor serial.
## 3.Loop

void loop() {
  byte error, address;
  int nDevices;
  Serial.println("Scanning...");
  nDevices = 0;
  for (address = 1; address < 127; address++) {
    // The i2c_scanner uses the return value of
    // the Write.endTransmission to see if
    // a device did acknowledge to the address.
    Wire.beginTransmission(address);
    error = Wire.endTransmission();
    Serial.println(address);
    if (error == 0) {
      Serial.print("I2C device found at address 0x");
      if (address < 16)
        Serial.print("0");
      Serial.print(address, HEX);
      Serial.println(" !");
      nDevices++;
    } else if (error == 4) {
      Serial.print("Unknown error at address 0x");
      if (address < 16)
        Serial.print("0");
      Serial.println(address, HEX);
    }
  }
  if (nDevices == 0)
    Serial.println("No I2C devices found\n");
  else
    Serial.println("done\n");
  delay(5000); // wait 5 seconds for next scan
}
En aquest loop, sent la part principal del programa, es fa un escaneig per veure els dispositius connectats al bus I2C, dintre d'aquest loop es diferencien diverses parts:

Variables:

'byte error, address;:' Declara variables per guardar errors y direccions I2C.
'int nDevices;:' Declara una altra variable per portar el conteig del número de dispositius I2C trobats.
Inicio del escaneo:

'Serial.println("Scanning...");:' Mostra per pantalla "Scanning..." al monitor serial.
'nDevices = 0;:' Inicialitza el conteig de dispositius a 0.
Bucle d'escaneig:

'address = 1; address < 127; address++' El bucle for passa per totes les direccions posibles de l'I2C (de 1 a 126).
'Wire.beginTransmission(address);:' Inicia una transmissió I2C a la direcció especificada.
'error = Wire.endTransmission();:' Termina la transmissió y retorna un codi d'error.
'Serial.println(address);:' Mostra per pantalla la direcció actual al monitor serial.
En cas de que no hi hagi error ('error == 0'):
Mostra per pantalla la direcció del dispositiu I2C trobat.
Augmenta el conteig del número de dispositius.
Si es detecta un error desconegut ('error == 4'):
Mostra per pantalla un missatge d'error indicant la direcció que dona problemes
Resultats de l'escaneig:

'if (nDevices == 0) Serial.println("No I2C devices found\n");:' Si no es troben dispositius, mostra el missatge "No I2C devices found\n".
'else Serial.println("done\n");:' Si es troben dispositius, mostra "done".
'delay(5000);:' Fa una pausa de 5000 milisegons (5 segons).

# Part B

Codi en Línea

#include <Arduino.h>

//YWROBOT
//Compatible with the Arduino IDE 1.0
//Library version:1.1
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27,20,4);  // set the LCD address to 0x27 for a 16 chars and 2 line display

void setup()
{
  Wire.begin(8,9);
  lcd.init();                      // initialize the lcd 
  // Print a message to the LCD.
  lcd.backlight();
  lcd.setCursor(3,0);
  lcd.print("Hello, world!");
  lcd.setCursor(2,1);
  lcd.print("Ywrobot Arduino!");
   lcd.setCursor(0,2);
  lcd.print("Arduino LCM IIC 2004");
   lcd.setCursor(2,3);
  lcd.print("Power By Ec-yuan!");
}


void loop()
{
}
Explicació del codi

## 1.Inclusió de llibreries

#include <Arduino.h>
#include <LiquidCrystal_I2C.h>
Aquí s'inclouen les llibreries per poder fer servir les funcions bàsiques d'Arduino i per poder controlar una pantalla LCD a través d'un bus I2C

## 2.Inicilització de pantalla LCD

LiquidCrystal_I2C lcd(0x27, 20, 4);  // set the LCD address to 0x27 for a 20 chars and 4 line display
Aquí es crea un objecte de classe 'LiquidCrystal_I2C' anomenat lcd amb la direcció I2C 0x27 especificant que la pantalla tindrà 4 files i 20 columnes.

## 3.Setup

void setup() {
  Wire.begin(8, 9);
  lcd.init();  // initialize the lcd
  lcd.backlight();
  lcd.setCursor(3, 0);
  lcd.print("Hello, world!");
  lcd.setCursor(2, 1);
  lcd.print("Ywrobot Arduino!");
  lcd.setCursor(0, 2);
  lcd.print("Arduino LCM IIC 2004");
  lcd.setCursor(2, 3);
  lcd.print("Power By Ec-yuan!");
}
'Wire.begin(8, 9);:' Aquí s'inicialitza la comunicació I2C al ESP32 utilitzant els pins GPIO 8 (SDA) i 9 (SCL).
'lcd.init();:' Inicialitza la pantalla LCD.
'lcd.backlight();:' Encen la llum del fons de la pantalla.
'lcd.setCursor(3, 0); lcd.print("Hello, world!");:' Coloca el cursor a la tercera columna i fila 0 y mostra el missatge "Hello, world!".
'lcd.setCursor(2, 1); lcd.print("Ywrobot Arduino!");:' Coloca el cursor segona columna i primera fila i mostra el missatge "Ywrobot Arduino!".
'lcd.setCursor(0, 2); lcd.print("Arduino LCM IIC 2004");:' Coloca el cursor columna zero i segona fila i mostra el missatge "Arduino LCM IIC 2004".
'lcd.setCursor(2, 3); lcd.print("Power By Ec-yuan!");:' Coloca el cursor segona columna i tercera fila i mostra el missatge "Power By Ec-yuan!".
## 4.Loop

void loop() {
}
En aquest codi la funció loop està buida, ja que l'objectiu és només el d'encendre la pantalla i mostrar els missatges un únic cop, sense necessitat de cap actualització.
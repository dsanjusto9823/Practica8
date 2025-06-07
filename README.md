# Practica8: Buses de Comunicación IV - UART
## Objetivo: 
El objetivo de esta práctica es comprender el funcionamiento de la comunicación serie asíncrona UART, permite la comunicación full-duplex asíncrona. 
En esta práctica se pide realizar un apartado aplicado a la comunicación UART, aunque debido a la simplicidad nos dicen que generamos nosotros el código de la práctica
## Apartado 1:
En este apartado queremos demostrar como enrturar datos de un microcontrolador a otro dispositivo utlizando UART2. Usaremos la conexión GPIO17 a GPIO16, con esta conexión al estar conectado a ESP32, nos facilita
la verificación y transmisión sin elementos externos
Los materiales usados han sido:
1. ESP32-S3
2. Cable Jumper
   ![image](https://github.com/user-attachments/assets/de9593e1-b777-439f-a883-27b26e179eeb)
### Código usado:
```
#include <Arduino.h>

#define RXD2 16  // Pin RX de UART2
#define TXD2 17  // Pin TX de UART2

void setup() {
  // Inicia comunicación serie con el PC
  Serial.begin(115200);

  // Inicia UART2 en los pines especificados
  Serial2.begin(115200, SERIAL_8N1, RXD2, TXD2);

  Serial.println("ESP32 UART2 Loop test");
}

void loop() {
  // Si hay datos desde el monitor serie, los envía por UART2
  if (Serial.available()) {
    char incoming = Serial.read();
    Serial2.write(incoming);
    Serial.print("UART0 -> UART2: ");
    Serial.println(incoming);
  }

  // Si hay datos devueltos desde UART2 (loopback), los imprime en el monitor
  if (Serial2.available()) {
    char incoming = Serial2.read();
    Serial.print("UART2 -> UART0: ");
    Serial.println(incoming);
  }
}
```
### Funcionamiento del código:
En este código usamos UART0 para que se comunique con el monitor serie del PC (por USB). Entonces todo lo que escribimos en el monitor serie se transmite a UART2. Y estos datos enviados se reciben otra vez en UART0,
por lo tanto en el monitor serie

###Salidas:
![image](https://github.com/user-attachments/assets/3babe9b0-0cd4-4434-830e-1543f11ce326)


[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=19574358&assignment_repo_type=AssignmentRepo)
# Lab04: Comunicación UART en PIC18F45K22

## Integrantes

[Juan Esteban Monroy Moya - 136851](https://github.com/Juanes20feb)

[Shirley Katherin Bohorquez Gil - 131164](https://github.com/Shirleyb0440)

[Alison Daniela Vera Rocha - 131212](https://github.com/Alisondaniela-bot)


## Documentación

### Introducción

En este laboratorio se implementa la comunicación serial UART utilizando el microcontrolador PIC18F45K22 en modo asíncrono. El objetivo es establecer una conexión entre el PIC y una computadora a través de un módulo USB-UART, permitiendo enviar y recibir datos en tiempo real. Esta práctica permite comprender la configuración del módulo EUSART y su utilidad en sistemas embebidos para tareas de monitoreo, control y depuración.

## Resultados

La implementación del laboratorio fue un éxito, ya que se logró establecer correctamente la comunicación serial entre el microcontrolador PIC18F45K22 y la computadora utilizando el módulo CP2102. Se pudieron enviar y recibir datos de forma confiable, lo que demuestra que la configuración del puerto serial, el manejo de registros y la lógica de transmisión y recepción fueron implementados de manera adecuada. Esto confirma el correcto funcionamiento del sistema y la comprensión de los principios de la comunicación UART.

### Visualización del código 

* Si deseas visualizar el código del programa principal [Haz clic aquí](main.c)

* Si deseas visualizar el código que contiene las funciones necesarias para configurar y operar la UART [Haz clic aquí](UART.c)

* Si deseas visualizar el código que contiene las funciones necesarias para configurar y operar el ADC [Haz clic aquí](ADC.c)

* Si deseas visualizar el código que contiene el encabezado con los prototipos de las funciones de la UART [Haz clic aquí](UART.h)

* Si deseas visualizar el código que contiene el encabezado con los prototipos de las funciones del ADC [Haz clic aquí](ADC.h)

### Explicación del código

#### Librerias utilizadas

`#include <xc.h>`: Librería del compilador XC8 para el PIC
`#include <stdio.h>`: Librería estándar de entrada/salida (para sprintf)
`#include "adc.h"`: Encabezado del módulo ADC
`#include "uart.h"`: Encabezado del módulo UART

#### Definición de la frecuencia del oscilador y configuración del microcontrolador

`#define _XTAL_FREQ 16000000`: Define que el PIC trabaja a 16 MHz
`#pragma config FOSC = INTIO67`: Oscilador interno habilitado
`#pragma config WDTEN = OFF`: Watchdog Timer deshabilitado
`#pragma config LVP = OFF`: Low Voltage Programming deshabilitado

#### Función principal

`void main(void) { `  
    `OSCCON = 0x72;` => Configura el oscilador interno a 16 MHz  
    `UART_Init();`   => Inicializa UART  
   ` ADC_Init(); `   =>Inicializa ADC  
    `while (1) {`  
        `unsigned int adc_value = ADC_Read(0);`   
        `float voltage = (adc_value * 5.0) / 1023.0;`   
        `char buffer[20];`  
        `sprintf(buffer, "Voltaje: %.2fV\r\n", voltage);`  
        `UART_WriteString(buffer);`  
        `__delay_ms(1000);}}`  

Dentro del ciclo `while(1)`, el programa realiza continuamente la adquisición y el envío del valor de voltaje: primero, lee el valor analógico del canal AN0 con `ADC_Read(0)`, que devuelve un número entero entre 0 y 1023 representando la señal analógica convertida en digital; después, este valor se convierte a un voltaje real usando la fórmula `(adc_value * 5.0) / 1023.0`, que ajusta la escala del ADC a 5V de referencia; luego, con `sprintf(buffer, "Voltaje: %.2fV\r\n", voltage)`, se forma una cadena de texto que contiene el voltaje con dos decimales, lista para enviar; a continuación, con UART_WriteString(buffer), se envía la cadena a través de la UART al dispositivo de destino (como una PC o un monitor serial); finalmente, el programa espera un segundo con `__delay_ms(1000)` antes de repetir el proceso, permitiendo que se visualicen las lecturas de forma ordenada y evitando que se saturen de mensajes el dispositivo receptor.

## Diagramas

## Implementación

![Implementación](Implementacion.png)

## Preguntas

1. ¿Para qué sirve la UART en un microcontrolador como el PIC18F45K22?

RTA: La UART (o EUSART en el PIC18F45K22) se utiliza para enviar y recibir datos de forma serial, lo que la convierte en un método muy popular para conectar el microcontrolador con otros dispositivos, como computadoras, sensores o módulos Bluetooth y WiFi, entre otros.

2. ¿Qué tipo de aplicaciones o implementaciones se pueden hacer con la UART?

RTA: Con la UART se pueden crear aplicaciones, tales como: Comunicación entre microcontroladores, conexión con módulos inalámbricos como Bluetooth, GSM o WiFi, monitoreo de sensores desde la computadora, enviar comandos desde la PC al microcontrolador y registrar datos en tiempo real.

3. ¿Cuál es la diferencia entre una comunicación serial síncrona y una asíncrona como la UART?

RTA: En la comunicación serial síncrona, los dispositivos comparten una señal de reloj que mantiene los datos sincronizados. En cambio, en la comunicación asíncrona (como la UART), no hay una señal de reloj común: cada dispositivo necesita estar configurado con la misma velocidad de transmisión (baud rate) para poder entenderse y sincronizarse bien.

4. ¿Qué registros del PIC18F45K22 se usan para transmitir y recibir datos con EUSART?

RTA: Para transmitir, se usa el registro TXREG, donde se escribe el dato que se desea enviar y para recibir, se usa el registro RCREG, desde donde se lee el dato recibido. Además, se utilizan banderas como TXIF (transmisión lista) y RCIF (dato recibido) para controlar el flujo de datos.

5. ¿Qué es <stdio.h>?

RTA: La librería `<stdio.h>` es un encabezado estándar de C que proporciona funciones esenciales para realizar operaciones de entrada y salida, como leer datos desde el teclado, imprimir texto en la consola y manipular archivos. Entre las funciones más usadas se encuentran `printf()` y `scanf()`, que permiten mostrar y recibir datos respectivamente, así como funciones como `fopen()` y `fclose()` para trabajar con archivos. En entornos de microcontroladores, algunas de estas funciones se adaptan a puertos de comunicación, como UART, ya que no siempre existe una consola de texto como en las PC.
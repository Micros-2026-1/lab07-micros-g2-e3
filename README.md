[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/rb0M7Pn8)
[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=23815528&assignment_repo_type=AssignmentRepo)
# Lab07: Visualización en LCD 16x2 usando módulo I²C con microcontrolador PIC

## Integrantes

[Samuel Forero](https://github.com/Sam232510)

[Danna Pineda](https://github.com/Danna-Pineda)

## Documentación

## Diagramas

![](montaje.png)

Figura 1. Montaje i2c más lcd
## Evidencias de implementación

![alt text](Fijo.jpeg)

Figura 2. Texto fijo "Hola mundo PIC18"

![alt text](Bateriai2c.gif)

Figura 3. Animación bateria con i2c.

![](Animacioni2c.gif)

Figura 4. Animación propia con i2c. 

![alt text](Scrolli2c.gif)

Figura 5. Texto con Scroll con i2c.
## Preguntas

1. ¿Por qué I²C se clasifica como half-duplex mientras que SPI es full-duplex? ¿Qué implicación práctica tiene esa diferencia para el control de una LCD?.

I²C se clasifica como un protocolo half-duplex porque utiliza una sola línea de datos (SDA) para transmitir y recibir información, por lo que la comunicación solo puede ocurrir en un sentido a la vez; en cambio, SPI es full-duplex debido a que posee líneas separadas para envío y recepción de datos (MOSI y MISO), permitiendo transmitir y recibir simultáneamente. En el control de una LCD, esto implica que I²C ofrece una comunicación más lenta pero con menos uso de pines del microcontrolador, siendo ideal para pantallas sencillas como las LCD 16x2, mientras que SPI permite una transferencia de datos mucho más rápida y eficiente, lo cual es ventajoso en pantallas gráficas o aplicaciones donde se requiere actualizar gran cantidad de información continuamente.


2. En I2C_init() se asigna SSPCON1 = 0x28. Desglose ese valor bit a bit e identifique qué modo de operación del MSSP se está seleccionando y por qué se elige ese valor.

El valor `SSPCON1 = 0x28` en hexadecimal corresponde a `00101000` en binario. Desglosando bit a bit: el bit 5 (`SSPEN = 1`) habilita el módulo MSSP para permitir la comunicación serial, mientras que los bits `SSPM3:SSPM0 = 1000` seleccionan el modo I²C Master. Los demás bits permanecen en 0 porque no se requiere detección de colisión ni otras funciones adicionales en esta configuración. Este valor se elige porque permite configurar el PIC como maestro I²C, encargado de generar la señal de reloj SCL y controlar la comunicación con dispositivos esclavos, como una LCD con adaptador I²C.

3. Las funciones I2C_start(), I2C_stop() e I2C_write() comparten el mismo patrón: activar un bit de control y luego esperar con while(!PIR1bits.SSPIF). ¿Qué representa la bandera SSPIF y por qué se limpia después de cada operación?.

La bandera `SSPIF` pertenece al registro `PIR1` y se activa cuando el módulo MSSP termina una operación de comunicación serial, como una condición Start, Stop, transmisión o recepción de datos en I²C. En las funciones `I2C_start()`, `I2C_stop()` e `I2C_write()`, el programa espera con `while(!PIR1bits.SSPIF)` hasta que el hardware indique que la operación finalizó correctamente. Después de cada operación, la bandera se limpia manualmente (`SSPIF = 0`) para preparar el módulo para la siguiente acción y evitar que el microcontrolador interprete una operación anterior como si fuera una nueva interrupción o evento completado.

4. El fuse PBADEN = OFF está presente en la configuración. ¿Qué efecto tendría dejarlo en ON sobre los pines del puerto B, y por qué podría causar problemas si se usan esos pines como salidas digitales?.

El fuse PBADEN = OFF desactiva la configuración analógica por defecto en los pines RB0 a RB4 del puerto B al iniciar el microcontrolador. Si se dejara en `ON`, esos pines arrancarían configurados como entradas analógicas en lugar de digitales, lo que impediría que funcionaran correctamente como salidas digitales. Como consecuencia, dispositivos conectados a esos pines, como LEDs, LCD o señales de control, podrían no responder, presentar niveles lógicos incorrectos o comportamientos inestables, ya que el módulo analógico toma prioridad sobre la lógica digital mientras no se reconfigure manualmente el puerto.

5. Compare el control de la LCD en modo paralelo (lab04) con el modo I²C de este laboratorio. Mencione ventajas y desventajas de cada enfoque en términos de: cantidad de pines usados, velocidad de actualización y complejidad del código.
6. El bus I²C permite conectar múltiples esclavos con solo dos hilos. Si se quisiera agregar un segundo módulo PCF8574 al mismo bus (por ejemplo, para controlar un segundo LCD), ¿qué cambio mínimo sería necesario en el hardware y en el código?

## Conclusiones

## Referencias
# Práctica 3: Controlador Máquina Expendedora
Sistema empotrados y de tiempo real
## Descripción de la práctica
  Se busca diseñar e implementar un controlador para una máquina expendedora que esté
  basado en Arduino UNO y en los sensores/actuadores que se proporcionan en el kit Arduino.
  La práctica tendrá que integrar obligatoriamente los siguientes componentes hardware:
  
  * Arduino UNO
  * LCD
  * Joystick
  * Sensor temperatura/Humedad DHT11
  * Sensor Ultrasonido
  * Boton
  * 2 LEDS Normales (LED1, LED2)

### Funcionalidad
La práctica debe implementar la siguiente funcionalidad software.
  1. Arranque:
     
      1.1 Al inicio del sistema, el LED1 debe parpadear 3 veces a intervalos de 1 segundo.
      Al mismo tiempo debe mostrarse el mensaje “CARGANDO …” en el LCD. Al
      cabo de los 3 parpadeos el LED1 debe apagarse y la pantalla debe mostrar la
      información de la funcionalidad “Servicio”.
  
  2. Servicio.
  
      2.1 Si el usuario se encuentra a menos de 1 metro de la máquina se debe pasar al
      estado b). En caso contrario el LCD debe mostrar “ESPERANDO CLIENTE”.
      
      2.2 El LCD debe mostrar la temperatura y humedad durante 5 segundos y acto
      seguido deberá mostrar la lista de productos que el usuario puede seleccionar.
      Los productos y precios a mostrar son:
      
      * i. Cafe Solo 1€
      * ii. Cafe Cortado 1.10 €
      * iii. Cafe Doble 1.25 €
      * iv. Cafe Premium 1.50 €
      * v. Chocolate 2.00 €
      
      Debes permitir la navegación por esa lista utilizando el joystick (arriba / abajo). Y
      para su selección debes usar el switch del propio joystick. Una vez seleccionado debes mostrar
      el mensaje “Preparando Cafe …” durante un tiempo aleatorio entre 4 y 8 segundos. Cada
      ejecución puede ser un tiempo distinto (debes hacerlo aleatorio). Usa ese mismo tiempo para
      hacer que el LED2 se encienda de manera incremental, de tal manera que la intensidad del
      LED2 indica igualmente el progreso de la preparación del café. Una vez terminada la
      preparación del cafe, el LDC debe mostrar “RETIRE BEBIDA” durante 3 segundos y volver a la
      funcionalidad inicial de Servicio.
      
      En cualquier momento del estado b), el usuario puede reiniciar el estado (no la
      placa) si pulsa el botón durante el rango 2-3 segundos. Por lo que deberá ejecutar de nuevo la
      funcionalidad de Servicio.
  
  3. Admin
     
      3.1 Es posible acceder a la interfaz de administración de la máquina en cualquier
      momento. Para ello el usuario debe presionar el botón durante no menos de 5
      segundos.
      
      3.2 Mientras el usuario esté en la vista de Admin, ambos LEDS deben estar
      encendidos.
      
      3.3 El siguiente menú debe ser mostrado en el LCD:
      
      * i. Ver temperatura
      * ii. Ver distancia sensor
      * iii. Ver contador
      * iv. Modificar precios
      
      Debes permitir la navegación por esa lista utilizando el joystick (arriba / abajo). Y
      para su selección debes usar el switch del propio joystick. Además debes permitir volver
      al menú utilizando el joystick (movimiento izquierda). A continuación se detalla lo que
      debe mostrar cada menú:
      
      * i) Temp: XX ºC Hum: YY % (debe cambiar dinámicamente)
      * ii) Distancia: XX cm (debe cambiar dinámicamente)
      * iii) Tienes que llevar un contador en segundos desde que la placa está
      arrancada. Ese contador en segundos es el que se muestra en este menú.
      Mientras estás en esa pantalla se tiene que observar como el contador va
      incrementando con el paso de los segundos.
      * iv) Debes mostrar el listado de productos y permitir el cambio de precio utilizando
      el joystick. Los incrementos o decrementos se realizan en 5 céntimos (utiliza el
      joystick arriba y abajo para cambiar el precio). Para confirmar el valor del precio
      utiliza el switch del joystick y para cancelar y volver a la lista de precio utiliza el
      movimiento “izquierda” del joystick. Ahora si vas a la lista ofrecida en la
      funcionalidad 2a) deberías ver los precios actualizados. (Los precios no
      persisten si la placa se reinicia).
      
      3.4 Para salir de la vista admin se debe pulsar de nuevo el pulsador durante no
      menos de 5 segundos, volviendo a la funcionalidad de Servicio.

## Funcionamiento de los sensores
### Ultrasonido
El funcionamiento básico del sensor de ultrasonido HC-SR04 con Arduino implica los siguientes pasos:

* Generación de un pulso ultrasónico: El Arduino envía una señal de pulso corto al pin de trigger (gatillo) del sensor HC-SR04. Este pulso de trigger es necesario para iniciar la medición de distancia.

* Emisión de ondas ultrasónicas: Cuando el pin de trigger recibe el pulso, el sensor envía una ráfaga de ondas ultrasónicas a través del transmisor ultrasónico.

* Recepción de las ondas reflejadas: Estas ondas rebotan en el objeto más cercano y son detectadas por el receptor ultrasónico del sensor.

* Cálculo de la distancia: El sensor calcula la distancia entre él y el objeto midiendo el tiempo que tarda en recibir las ondas ultrasónicas reflejadas. Utilizando la fórmula de distancia = velocidad × tiempo / 2 (ya que la onda va y viene), se estima la distancia.

![image](https://github.com/cescarcena2021/Controlador-Maquina-Expendedora/assets/102520602/8ed6505c-8bf7-4985-8aef-da9cbafd7fdf)

* Lectura de la distancia por Arduino: El Arduino lee la duración del pulso de eco que recibe del pin de eco del sensor y, mediante cálculos, convierte esta duración en una distancia.
``` Arduino
void setup() {
  Serial.begin(9600);
  pinMode(Trigger, OUTPUT);
  pinMode(Echo, INPUT);
}

void loop() {
  long tiempo_echo, distancia;
  
  //lanzar el trigger
    digitalWrite(Trigger, HIGH);
    delayMicroseconds(10);          //Enviamos un pulso de 10us
    digitalWrite(Trigger, LOW);
    
    tiempo_echo = pulseIn(Echo, HIGH); 
    distancia = tiempo_echo/59;  //distancia en cm
```
  
### Joystick

El joystick generalmente está conectado a través de varios pines a una placa Arduino. Los dos potenciómetros están conectados a dos pines analógicos (por ejemplo, A0 y A1), y el botón de presión central, si lo tiene, se conecta a un pin digital.

El funcionamiento básico de un joystick en Arduino implica leer los valores analógicos proporcionados por los potenciómetros para determinar la posición del joystick en los ejes X e Y, y leer el estado del botón (si está presente) para detectar si ha sido presionado o no.

![image](https://github.com/cescarcena2021/Controlador-Maquina-Expendedora/assets/102520602/c812112d-59b6-4c19-96a0-068abace7720)

### Display LCD
Un LCD (Liquid Crystal Display) conectado a Arduino permite mostrar información de texto y/o gráficos de una manera legible y visualmente clara. El funcionamiento básico implica enviar comandos y datos al LCD para controlar qué se muestra en la pantalla. Para trabajar con un LCD en Arduino, se utiliza una librería como "LiquidCrystal.h" que facilita el control del LCD.

![image](https://github.com/cescarcena2021/Controlador-Maquina-Expendedora/assets/102520602/89ede3c4-ae6e-4617-ad35-28c362c40f30)

### Sensor temperatura/Humedad DHT11
El funcionamiento del sensor DHT11 es relativamente sencillo:

* Medición de la temperatura: El termistor dentro del sensor detecta cambios de resistencia en función de la temperatura ambiente. Esta variación de resistencia se convierte en una señal digital que el sensor DHT11 puede leer.

* Medición de la humedad: El sensor de humedad capacitivo mide la variación en la capacitancia del material sensible a la humedad dentro del sensor. Esta variación se convierte en una señal digital que indica el nivel de humedad.

* Salida de datos: El sensor DHT11 convierte las mediciones analógicas de temperatura y humedad en señales digitales que se pueden leer a través de un solo pin de datos. Utiliza un protocolo propio para comunicar estos datos al microcontrolador al que está conectado (como Arduino).

* Para utilizar un sensor DHT11 con Arduino, hay una libreria para facilitar la lectura de datos del sensor. La librería "DHT.h" proporciona funciones que permiten leer la temperatura y la humedad del sensor DHT11.

* Además de eso en el kit proporcionado por la universidad podemos encontrar el mismo sensor con difernetes consexiones, en mi caso me toco el de la izquierda, que tiene 3 patas correspondientes a pin de datos, voltaje(VCC) y tierra(GND) respectivamente

  ![image](https://github.com/cescarcena2021/Controlador-Maquina-Expendedora/assets/102520602/9e8591f4-ed27-4f15-a356-ec3057f83339)

### Implemantación 
Para hacer esta practica, en primer lugar decidí hacer un esquema de como seria le circuito, a si que decidí hacer un sketch en Fritzing. Tube algunos problemas al principo ya que no disponia de pines digitales suficientes para conectar todos los sensores, a si que decidi conectar uno de los led a un pin analogico. Esto es poslible ya que este es capaz de recivir señales analogicas y justo ese led nu requiere de **pulse width modulation** (PWM). Ademas, otro de los problemas que tube fu respecto al sensor de temeratura que el que yo tenia no seguia el esquma de pines combencional y tarde bastante haste que descubri que el problema no estaba en el sensor ni el codigo si no en la conexion de los pines
![image](https://github.com/cescarcena2021/Controlador-Maquina-Expendedora/assets/102520602/eeb1fbeb-c230-45fb-bf0b-6490b5d89b53)

### Chasis 
Esta practica requiere de varios sensores y varios de ellos son interactuables como el boton o el joystick,. Mi escasa experiencia en arduino me decia que cada vez que quisiare depurar la practca y usara cualquiera de estos interactuables, por error algun cable se soltaría y tardaria horas en saber donde estaba conactado. Para evitar estos problemas se me ocurrio construir un chasis que encapsulara todos los sensores y consexiones y dejara una fachada muscho mas elegante de cara el usuario.
Para ellos utilice alunos de los legos que tenia por casa y me puse manos a la obra. Anclé tanto la protoboard como la placa de Arduino a la base y genere unas parades de poco tamaño para que todo estubiera seguro. Mas tarde comence a realizar las conexiones y a colocar los sensores en sus corespondientes posiciones. Una vez terminada toda la parte electronica continue con la contrucion del chasis para que la maquina quedara totalmente sellada. Mas tarde añadi una entrada para conetar la placa a corriente y un pulsador que se conecta con el boton. Y finalmente este fue el resultado final..

![image](https://github.com/cescarcena2021/Controlador-Maquina-Expendedora/assets/102520602/ff1acc87-a9e5-44fc-b7b7-2e78835186db)
![image](https://github.com/cescarcena2021/Controlador-Maquina-Expendedora/assets/102520602/2d77b338-f76f-4186-a007-d05b1d96fc12)

### Documentación:

* https://www.arduino.cc/reference/en/
  
Bibliotecas utilizadas:

* LiquidCrystal: https://www.arduino.cc/en/Reference/LiquidCrystal
* ArduinoThread: https://www.arduino.cc/reference/en/libraries/arduinothread/
* Watch Dog: https://create.arduino.cc/projecthub/rafitc/what-is-watchdog-timer-fffe20
* DHT-sensor-library: https://github.com/adafruit/DHT-sensor-library
* TimerOne: https://www.arduino.cc/reference/en/libraries/timerone/

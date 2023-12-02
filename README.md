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

[Enunciado de la practica](https://github.com/cescarcena2021/Controlador-Maquina-Expendedora/blob/master/Practica3_MaquinaExpendedora.pdf)


## Funcionamiento de los sensores
### Ultrasonido
El funcionamiento básico del sensor de ultrasonido HC-SR04 con Arduino implica los siguientes pasos:

* Generación de un pulso ultrasónico: El Arduino envía una señal de pulso corto al pin de trigger (gatillo) del sensor HC-SR04. Este pulso de trigger es necesario para iniciar la medición de distancia.

* Emisión de ondas ultrasónicas: Cuando el pin de trigger recibe el pulso, el sensor envía una ráfaga de ondas ultrasónicas a través del transmisor ultrasónico.

* Recepción de las ondas reflejadas: Estas ondas rebotan en el objeto más cercano y son detectadas por el receptor ultrasónico del sensor.

* Cálculo de la distancia: El sensor calcula la distancia entre él y el objeto midiendo el tiempo que tarda en recibir las ondas ultrasónicas reflejadas. Utilizando la fórmula de distancia = velocidad × tiempo / 2 (ya que la onda va y viene), se estima la distancia.

![image](https://github.com/cescarcena2021/Controlador-Maquina-Expendedora/assets/102520602/8ed6505c-8bf7-4985-8aef-da9cbafd7fdf)

* Lectura de la distancia por Arduino: El Arduino lee la duración del pulso de eco que recibe del pin de eco del sensor y, mediante cálculos, convierte esta duración en una distancia.
```c++
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

### Código

Para la parte software de esta practica comece haciendo un esquema a mano de como seria la estructurta principal del codigo y que pines tenia que conectar en que sitios ya que estaban muy justos. Una vez logre terminar el boceto me puse manos a la obra definiendo las constantes y crando las principales funciones de lectura de datos de los sensores como por ejemplo **ver_temperatura()** o **ver_distancia()**. Mas tarde añadí la navegacion por el menu de productos, para el cual use una pequeña *struct* que me definí para que me fuera mas facil. Tambien inicialice todo debidamente en el *setup()*.
```c++
struct Producto{
  float precio;
  String modelo;
};
```

Para el bucle principal use una estructura *switch case* en la cual en funcion de las lectura del joystick entratiamos en un lugar o en otro. Además de eso he usado varios condicionales para saber si la maquina necesita imprimir el menu de usuario o el de administrador, para saber si el cliente se encuentra cerca o para saber si la maquina se encuantra en estado de arranque. Tambien decidi añadir trazas en varios sitios para que en un futuro si la maquina se rompe, la depuracion se mas facil.
```c++
choice = leer_joistick();
  //Serial.println(choice);
  if(choice != last_choice){
    lcd.clear();
    last_choice = choice;
    switch(choice){
      case up:
        move_up();
        Serial.println("up");
        break;
      case down:
        move_down();
        Serial.println("down");
        break;
      case left:
        move_left();
        Serial.println("left");
        break;
      case enter:
        press_button();
        Serial.println("button");
        break;
      default: 
        break;
    }
```
Para entar el es modo adminitrador y para reinicir la maquina era necesario hacer uso de las pulsaciones de un boton en cualquier momento y estado de la maquina. Así que decidí usar las interupciones para ello. Genere un interrupcion que saltara cuando el boton cambia de estado y si este esta siendo presionado guarda el tiempo de inicio de la pulsacion, y si justo se acaba de soltar gurada el tiempo del final de la pusacion. Con estos dos valores somos capaces de calcular el tiempo de la puslacion y con ello ya podremos decidir si queremos reiniciar la maquina(entre 2 y 3 segundos) o si quermos entrar en el estado de adminsitrador(mas de 5 segundos). Ademas añadi un condicional al comienzo para eliminar las falsas pulsaciones.
```c++
void botonInterrupcion(){

  //eliminamos el rebote
  if (millis() - startTime > timeThreshold)
  {
    startTime = millis();

    if (digitalRead(Pin_Button) == LOW) { // Verifica si el botón está siendo pulsado
      Inicio = millis(); // Guarda el tiempo en el que se inició la pulsación
    }
    if (digitalRead(Pin_Button) == HIGH) { // Verifica si el botón se ha dejado de pulsar
      Final = millis(); // Guarda el tiempo del final de la pulsación
    }
    total = Final - Inicio;
  }
}
```
Para el menu de adminsitrador tambien he usado una estructuctura *switch case* pero esta vez en vez de con las posobles posiciones del joystick, con las posibles opciones del menu de admunistrador. De este modo he consegido poder estar dntro de la misma opcion dirante el tiempo que queramos sin parar el ciclo del bucle principal. Para salir de la opcion seleciona solo basta con mover el joystick a la izquierda y la variable *salir* se pondra a false para que no se ejecute el *switch case*. 
```c++
if(!salir){
      switch(current_choice){
        case ver_temperatura_:
          ignore = true;
          ver_temperatura();
          break;
        case ver_sensor_dist_:
          ignore = true;
          ver_sensor_distancia();
          break;
        case ver_contador_:
          ignore = true;
          ver_contador();
          break;
        case modificar_precios_:
          modificar_precios();
          break;
        default: 
          break;
      }
    }else{
      show_admin_menu(current_choice);
    }
```

Finalmente para hacer mas robusto el codigo he añadido un watchdog. Este lo inicializmos en el *setup()* a 8 segundos y lo resetamos al final del *loop()*. Ademas tambien es necesario reciniarlo en la funcion del led progresivo, ya que este hace un delay entre 4 y 8 y es possible que se reinice cuando no deberia en ese caso, por es justo despues de salir del *for* que enciende el led, cuando volvemos a reseatear el watchdog.
```c++
void setup() {
  //Inicializar el watchdog
  wdt_disable();
  wdt_enable(WDTO_8S);
}
...

void progresive_led(int time_){

  //encender progresivamente
  for (int brillo = 0; brillo <= 255; brillo++) {
    analogWrite(ledVerde, brillo);
    delay(time_ / 256); 
  }

  //Cuando termina apaga el led y reseatamos el wdt para que no salte 
  analogWrite(ledVerde, LOW);
  wdt_reset();
}

...

void loop(){

  ...

  //al final de bucle resetamos para que no salte el watchdog
  delay(100);
  wdt_reset();
}
```
### Documentación:

* https://www.arduino.cc/reference/en/
  
Bibliotecas utilizadas:

* LiquidCrystal: https://www.arduino.cc/en/Reference/LiquidCrystal
* ArduinoThread: https://www.arduino.cc/reference/en/libraries/arduinothread/
* Watch Dog: https://create.arduino.cc/projecthub/rafitc/what-is-watchdog-timer-fffe20
* DHT-sensor-library: https://github.com/adafruit/DHT-sensor-library
* TimerOne: https://www.arduino.cc/reference/en/libraries/timerone/

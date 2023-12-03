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

Para leer los datos de este sensor y traducirlos a movimiento he creado la siguiente funcion:

```c++
int leer_joistick(){
  //izquierda [0], derecha [1]
  //arriba [2], abajo [3]
  //boton pulsado [4]

  int VRx = analogRead(Pin_x);
  int VRy = analogRead(Pin_y);

  int R3 = digitalRead(joistick_Button);

  if(R3 != 1){
    return enter;  //boton pulsado
  }
  
  if(VRx > 700){
    return up;  //arriba
  }else if(VRx < 200){
    return down; //abajo
  }

  if(VRy > 700){
    return right;  //izquierda
  }else if(VRy < 200){
    return left;  //derecha
  };

  return -1;
  
}
```

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
Para hacer esta práctica, en primer lugar decidí hacer un esquema de cómo sería el circuito, así que decidí hacer un sketch en Fritzing. Tuve algunos problemas al principio ya que no disponía de pines digitales suficientes para conectar todos los sensores, así que decidí conectar uno de los leds a un pin analógico. Esto es posible ya que este es capaz de recibir señales analógicas y justo ese led no requiere de *pulse width modulation* (PWM). Además, otro de los problemas que tuve fue respecto al sensor de temperatura, ya que el que yo tenía no seguía el esquema de pines convencional y tardé bastante hasta que descubrí que el problema no estaba en el sensor ni en el código, sino en la conexión de los pines
![image](https://github.com/cescarcena2021/Controlador-Maquina-Expendedora/assets/102520602/eeb1fbeb-c230-45fb-bf0b-6490b5d89b53)

### Chasis 
Esta práctica requiere de varios sensores y varios de ellos son interactivos como el botón o el joystick. Mi escasa experiencia en Arduino me decía que cada vez que quisiera depurar la práctica y usara cualquiera de estos interactivos, por error algún cable se soltaría y tardaría horas en saber dónde estaba conectado. Para evitar estos problemas se me ocurrió construir un chasis que encapsulara todos los sensores y conexiones y dejara una fachada mucho más elegante de cara al usuario. Para ello utilicé algunos de los Legos que tenía por casa y me puse manos a la obra. Anclé tanto la protoboard como la placa de Arduino a la base y generé unas paredes de poco tamaño para que todo estuviera seguro. Más tarde comencé a realizar las conexiones y a colocar los sensores en sus correspondientes posiciones. Una vez terminada toda la parte electrónica continué con la construcción del chasis para que la máquina quedara totalmente sellada. Más tarde añadí una entrada para conectar la placa a corriente y un pulsador que se conecta con el botón. Y finalmente este fue el resultado final..

![image](https://github.com/cescarcena2021/Controlador-Maquina-Expendedora/assets/102520602/ff1acc87-a9e5-44fc-b7b7-2e78835186db)
![image](https://github.com/cescarcena2021/Controlador-Maquina-Expendedora/assets/102520602/2d77b338-f76f-4186-a007-d05b1d96fc12)

### Código

Para la parte software de esta práctica comencé haciendo un esquema a mano de cómo sería la estructura principal del código y qué pines tenía que conectar en qué sitios, ya que estaban muy justos. Una vez logré terminar el boceto me puse manos a la obra definiendo las constantes y creando las principales funciones de lectura de datos de los sensores como por ejemplo *ver_temperatura()* o *ver_distancia()*. Más tarde añadí la navegación por el menú de productos, para el cual usé una pequeña struct que me definí para que me fuera más fácil. También inicialicé todo debidamente en el *setup()*.
```c++
struct Producto{
  float precio;
  String modelo;
};
```

Para el bucle principal usé una estructura switch case en la cual, en función de las lecturas del joystick, entraríamos en un lugar o en otro. Además de eso, he usado varios condicionales para saber si la máquina necesita imprimir el menú de usuario o el de administrador, para saber si el cliente se encuentra cerca o para saber si la máquina se encuentra en estado de arranque. También decidí añadir trazas en varios sitios para que en un futuro si la máquina se rompe, la depuración sea más fácil.
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
Para entrar en el modo administrador y para reiniciar la máquina era necesario hacer uso de las pulsaciones de un botón en cualquier momento y estado de la máquina. Así que decidí usar las interrupciones para ello. Generé una interrupción que saltara cuando el botón cambia de estado y si este está siendo presionado guarda el tiempo de inicio de la pulsación, y si justo se acaba de soltar guarda el tiempo del final de la pulsación. Con estos dos valores somos capaces de calcular el tiempo de la pulsación y con ello ya podremos decidir si queremos reiniciar la máquina (entre 2 y 3 segundos) o si queremos entrar en el estado de administrador (más de 5 segundos). Además, añadí un condicional al comienzo para eliminar las falsas pulsaciones.

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
Para el menú de administrador también he usado una estructura switch case pero esta vez en vez de con las posibles posiciones del joystick, con las posibles opciones del menú de administrador. De este modo he conseguido poder estar dentro de la misma opción durante el tiempo que queramos sin parar el ciclo del bucle principal. Para salir de la opción seleccionada solo basta con mover el joystick a la izquierda y la variable salir se pondrá a false para que no se ejecute el switch case
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

Finalmente, para hacer más robusto el código, he añadido un watchdog. Este lo inicializamos en el *setup()* a 8 segundos y lo reseteamos al final del *loop()*. Además, también es necesario reiniciarlo en la función del led progresivo, ya que este hace un delay entre 4 y 8, y es posible que se reinicie cuando no debería. En ese caso, es justo después de salir del *for* que enciende el led, cuando volvemos a resetear el watchdog.

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

## Video demostrativo 
En el siguiente video se puede ver perfectamente el funcionamiento completo de la maquina entrando en los diferentes menus y y difrentes opciones.
https://youtu.be/lbYajS9Zx08

### Documentación:

* https://www.arduino.cc/reference/en/
  
Bibliotecas utilizadas:

* LiquidCrystal: https://www.arduino.cc/en/Reference/LiquidCrystal
* ArduinoThread: https://www.arduino.cc/reference/en/libraries/arduinothread/
* Watch Dog: https://create.arduino.cc/projecthub/rafitc/what-is-watchdog-timer-fffe20
* DHT-sensor-library: https://github.com/adafruit/DHT-sensor-library
* TimerOne: https://www.arduino.cc/reference/en/libraries/timerone/

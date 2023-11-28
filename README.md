# Práctica 3: Controlador Máquina Expendedora
Sistema empotrados y de tiempo real
## Descripción de la práctica
Se busca diseñar e implementar un controlador para una máquina expendedora que esté
basado en Arduino UNO y en los sensores/actuadores que se proporcionan en el kit Arduino.
La práctica tendrá que integrar obligatoriamente los siguientes componentes hardware:
● Arduino UNO
● LCD
● Joystick
● Sensor temperatura/Humedad DHT11
● Sensor Ultrasonido
● Boton
● 2 LEDS Normales (LED1, LED2)
Funcionalidad
La práctica debe implementar la siguiente funcionalidad software.
1. Arranque:
a. Al inicio del sistema, el LED1 debe parpadear 3 veces a intervalos de 1 segundo.
Al mismo tiempo debe mostrarse el mensaje “CARGANDO …” en el LCD. Al
cabo de los 3 parpadeos el LED1 debe apagarse y la pantalla debe mostrar la
información de la funcionalidad “Servicio”.
2. Servicio.
a. Si el usuario se encuentra a menos de 1 metro de la máquina se debe pasar al
estado b). En caso contrario el LCD debe mostrar “ESPERANDO CLIENTE”.
b. El LCD debe mostrar la temperatura y humedad durante 5 segundos y acto
seguido deberá mostrar la lista de productos que el usuario puede seleccionar.
Los productos y precios a mostrar son:
i. Cafe Solo 1€
ii. Cafe Cortado 1.10 €
iii. Cafe Doble 1.25 €
iv. Cafe Premium 1.50 €
v. Chocolate 2.00 €
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
a. Es posible acceder a la interfaz de administración de la máquina en cualquier
momento. Para ello el usuario debe presionar el botón durante no menos de 5
segundos.
b. Mientras el usuario esté en la vista de Admin, ambos LEDS deben estar
encendidos.
c. El siguiente menú debe ser mostrado en el LCD:
i. Ver temperatura
ii. Ver distancia sensor
iii. Ver contador
iv. Modificar precios
Debes permitir la navegación por esa lista utilizando el joystick (arriba / abajo). Y
para su selección debes usar el switch del propio joystick. Además debes permitir volver
al menú utilizando el joystick (movimiento izquierda). A continuación se detalla lo que
debe mostrar cada menú:
● i) Temp: XX ºC Hum: YY % (debe cambiar dinámicamente)
● ii) Distancia: XX cm (debe cambiar dinámicamente)
● iii) Tienes que llevar un contador en segundos desde que la placa está
arrancada. Ese contador en segundos es el que se muestra en este menú.
Mientras estás en esa pantalla se tiene que observar como el contador va
incrementando con el paso de los segundos.
● iv) Debes mostrar el listado de productos y permitir el cambio de precio utilizando
el joystick. Los incrementos o decrementos se realizan en 5 céntimos (utiliza el
joystick arriba y abajo para cambiar el precio). Para confirmar el valor del precio
utiliza el switch del joystick y para cancelar y volver a la lista de precio utiliza el
movimiento “izquierda” del joystick. Ahora si vas a la lista ofrecida en la
funcionalidad 2a) deberías ver los precios actualizados. (Los precios no
persisten si la placa se reinicia).
d. Para salir de la vista admin se debe pulsar de nuevo el pulsador durante no
menos de 5 segundos, volviendo a la funcionalidad de Servicio.
Recomendaciones
● Haz uso de todas las librerías y técnicas vistas en clase.
● Mantén tu código seguro utilizando el watchdog para evitar bloqueos.
Entrega definida en aula virtual.
Documentación:
● https://www.arduino.cc/reference/en/
Bibliotecas utilizadas:
● LiquidCrystal: https://www.arduino.cc/en/Reference/LiquidCrystal
● ArduinoThread: https://www.arduino.cc/reference/en/libraries/arduinothread/
● Watch Dog: https://create.arduino.cc/projecthub/rafitc/what-is-watchdog-timer-fffe20
● DHT-sensor-library: https://github.com/adafruit/DHT-sensor-library
● TimerOne: https://www.arduino.cc/reference/en/libraries/timerone/

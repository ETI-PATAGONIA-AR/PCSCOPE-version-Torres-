Hace poco llegue por casualidad al siguiente post: https://www.forosdeelectronica.com/threads/aporte-osciloscopio-en-base-a-una-pc-con-arduino.169001/
y me parecio muy buen aporte este diseño de osciloscopio ARDUINO que trabaja via serial con un software que corre en Window... El proyecto esta basado en 
EFY PC Oscilloscope (Balaji Ramalingam, Bosch) y se me ocurrio hacer una version que permita usar un oled, o el puerto serial con el software... 
En realidad hice varias versiones y las que mas se amoldan a algo amateur/aficionado, son estas versiones:

**La 1.3 es solo OLED**
<img width="924" height="562" alt="pcscope_v1_3" src="https://github.com/user-attachments/assets/bf80a70c-2dc5-4fe1-952b-96934dadf1f4" />

**La 1.3b es DUAL (OLED o salida serial al software)**
<img width="926" height="561" alt="pcscope_v1_3b" src="https://github.com/user-attachments/assets/cfcd5c3b-b2ce-4780-a64a-cdecee9ef0ab" />

**La 1.4 es solo OLED**
<img width="888" height="548" alt="pcscope_v1_4" src="https://github.com/user-attachments/assets/8052dcc9-e89a-467d-a705-d5fcad14d1cb" />

_**Tengan en cuenta que la version 1.4b es DUAL (OLED o salida serial al software), pero no se puede usar en proteus**_
Bien, ahora de seguro que se preguntaran por que la versión 1.4b no funciona en proteus… Antes que nada, trate de mantener la idea intacta de tener algo ágil 
y que este dentro del ancho de banda del proyecto original... Para poder llegar a la consigna, implemente la librería U8g2 que es mucho más liviana en RAM y 
flash, especialmente en micros tipo ATmega328 (Arduino UNO / Nano); Además, el SH1106/ SSD1306 funciona más rápido y más estable con esa librería, ya que 
Adafruit_SH110X tiende a ser más pesada.
Ahora, no es problema tuyo o mio de no tener una PC cuantica. Proteus no es un software que puede hacer todo lo que queremos, ya que es tan solo un modulo de 
captura esquemática dentro de una suite de diseño electrónico, que por medio de modelos matemáticos nos resuelve y permite recrear "algunos proyectos"; Pero 
esos modelos no son perfectos, y muchas veces el típico error de tiempos en simulación nos generan fallos o directamente no nos permiten simular nada. En este 
caso en particular, todas las versiones que no comparti, falla ISIS porque introduce el modo “Free Running + ISR(ADC_vect)”, que basicamente dicho en palabras 
criollas: "Proteus no soporta correctamente". Se sobre entiende que jamás va a simular en tiempo real algún proyecto, mas que nada es para hacer algunos debug 
e ir parchando y mejorando nuestros proyectos en fase de desarrollo... La version 1.4b pude solucionar en físico un gran problema que tenia. Origianlmente 
mantenia el ADC funcionando en modo automático con interrupciones (ADATE + ADIE) para el modo Serial, pero cuando pasaba al modo OLED, seguía haciendo lecturas 
con analogRead() sin detener el ADC en modo libre. Esto causa un conflicto directo entre:

_el hardware ADC que ya está midiendo continuamente en el ISR,y las lecturas bloqueantes de analogRead() en captureWaveform()._

O sea, el resultado que tenia era que el bus ADC se trababa y la pantalla nunca actualizaba obteniendo un OLED que queda negro o congelado.
Se que hay un montón de sketch de osciloscopio Arduino, pero me gusto este proyecto que ya viene acompañado de un software...
La version "pcscope Torres", tanto la opción serial como la opción OLED, tiene los 5khz que originalmente ofrecen, y para la gran mayoría de los trabajos de talleres 
de inyección electrónica, creo que seria una muy buena herramienta de diagnostico (si lo saben usar)…
Decir que no tengo a mano un oled grande, sino armaba algo mas lindo.... Igualmente, si lo piensan, con un nano, este oled y el minimo hardware que lleva, quedaria 
algo super portatil 

**Les adjunto tambien el archivo STL para que pueda imprimir el gabinete:**

<img width="1365" height="720" alt="Captura de pantalla 2025-11-12 123036" src="https://github.com/user-attachments/assets/140cca06-a9ad-4a66-a1f4-3cbd7a62b13b" />

<img width="1364" height="712" alt="Captura de pantalla 2025-11-12 125144" src="https://github.com/user-attachments/assets/47267f96-e11b-4f68-9ab0-9478923b5f17" />

<img width="1365" height="716" alt="Captura de pantalla 2025-11-12 125246" src="https://github.com/user-attachments/assets/d35665e6-abeb-4fb4-933a-af114c73c158" />

<img width="1365" height="708" alt="Captura de pantalla 2025-11-12 125324" src="https://github.com/user-attachments/assets/06ca9bff-2e7c-4a79-af05-8b449996235e" />

<img width="1364" height="714" alt="Captura de pantalla 2025-11-12 125525" src="https://github.com/user-attachments/assets/5143e7ef-3778-4576-9313-f04fab4fb18f" />

Un pequeño video de la re-versión funcionando ( https://www.youtube.com/watch?v=o8JcZfujk9g )...

[![CtrlVOZ_VR3_v2](https://img.youtube.com/vi/o8JcZfujk9g/0.jpg)](https://www.youtube.com/watch?v=o8JcZfujk9g)







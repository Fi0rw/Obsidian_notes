**Electronic Workbench (EWB)** es un software de simulación de circuitos electrónicos. Permite armar circuitos virtuales y ver su comportamiento con instrumentos virtuales (osciloscopio, multímetro, generador de señal, etc.) **sin necesidad de componentes físicos reales.**

![[simulador-1 1.png]]

Se observa un circuito **sin filtro** con un generador senoidal de $1kHz$ y una amplitud de $10V$ aplicada sobre una resistencia de $1k\Omega$

--- 

## Generador de Función
Es el instrumento que **genera la señal eléctrica** que se aplica al circuito. Es el equivalente virtual del $T_{x}$ (transmisor). En la ventana del simulador se configuran:

![[function-generator.png]]

| Parámetro      | ¿Qué controla?                              | En el experimento          |
| -------------- | ------------------------------------------- | -------------------------- |
| **Frequency**  | La frecuencia de la señal generada          | Varía entre 100 Hz y 1 MHz |
| **Amplitude**  | La amplitud de la señal (voltaje pico)      | 10 V en los ejemplos       |
| **Duty Cycle** | Solo para señales cuadradas, no aplica aquí | 50% (no se modifica)       |
| **Offset**     | Desplazamiento en CC de la señal            | 0 (no se modifica)         |

---

## Osciloscopio
El osciloscopio es el instrumento que **muestra visualmente la forma de onda** de la señal eléctrica, muestra gráficamente cómo varía el voltaje en el tiempo. El eje horizontal es el tiempo, el vertical el voltaje. En el contexto del simulador, muestra la señal que llega al receptor ($R_{x}$)

![[osciloscopio.png]]

| Parámetro         | ¿Qué es?                                                                                                         |
| ----------------- | ---------------------------------------------------------------------------------------------------------------- |
| **Time base**     | Escala de tiempo horizontal (ej: 0.50ms/div significa que cada cuadradito del eje X representa 0.5 milisegundos) |
| **Channel A / B** | Los dos canales de entrada. En los experimentos solo se usa canal A.                                             |
| **V/Div**         | Escala de voltaje vertical (ej: 5 V/Div significa que cada cuadradito del eje Y representa 5 V)                  |
**¿Cómo se lee la amplitud?** Se cuentan cuántos cuadraditos de alto tiene la onda y se multiplica por el valor de V/Div.

> **Ejemplo:** Con 5 V/Div, si la onda mide 2 cuadraditos de alto → amplitud = 2 × 5 = 10 V.


--- 

## Filtro Pasa Altos en el Simulador
Un **filtro pasa alto de segundo orden** con $C$ (capacitor) en serie y $L$ (inductor) en derivación.

Se presenta un circuito con un condensador de $10uF$ y un inductor de $1mH$. Una **frecuencia** de $500Hz$, se observa una **baja amplitud** en el osciloscopio.

![[simulador-2.png]]

Se **aumenta la frecuencia** a $1kHz$ y la **amplitud de la señal cambia**. Se observa en el osciloscopio que la amplitud llega a los $5V$ aproximadamente (está configurado para que cada división/recuadro del osciloscopio sea $5V$)

![[simulador-3.png]]

Se **aumenta la frecuencia** nuevamente, pero ahora a $1,5kHz$ y se observa en el osciloscopio que la amplitud aumenta a aproximadamente $10V$ 

![[simulador-4.png]]

Se puede concluir que efectivamente es un **filtro pasa altos**. A medida que la frecuencia aumenta, la señal en el receptor ($R_{x}$) aumenta. Por otro lado, con menor frecuencia (por debajo de $f_{ci}$) la amplitud presenta atenuación. 

---

## Filtro Pasa Bajos en el Simulador
Un **filtro pasa bajos de segundo orden** con $L$ (inductor) en serie y $C$ (capacitor) en derivación.

Se presenta un circuito con un inductor de $1mH$ y un capacitor de $1uF$. Una **frecuencia** de $14KHz$, se observa una **muy baja amplitud** de la señal en el osciloscopio.

![[simulador-1 2.png]]

Se disminuye la frecuencia a $7,7KHz$ y se observa que la amplitud aumenta pero sin llegar a los $10V$

![[simulador-2 1.png]]

Al disminuir la frecuencia a $1KHz$ se observa que el receptor recibe la señal completa. La amplitud llega a los $10V$, la amplitud máxima.

![[simulador-3 1.png]]

Al disminuir aún más la frecuencia a $500Hz$ la amplitud de la señal no disminuye, se mantiene en $10V$. 

![[simulador-4 1.png]]

Se puede concluir que efectivamente es un **filtro pasa bajos**. A medida que la frecuencia disminuye, la señal en el receptor ($R_{x}$) aumenta. Sin embargo, al superar la **frecuencia de corte ($f_{c}$​)**, la reactancia del circuito aumenta la oposición al paso de la corriente, provocando que la amplitud de la señal se atenúe progresivamente a medida que la frecuencia sube. 

---

## Filtro Pasa Banda
El circuito es un **RLC serie**: $L$ (Inductor) y $C$ (condensador) en serie y la resistencia ($R$) en derivación. El inductor de $1mH$, el condensador de $1uF$ y la resistencia de $1k\Omega$. 

Si se calcula la **frecuencia de resonancia** $f_{0} = \frac{1}{2\pi \sqrt{ LC }}$ daría como resultado que la señal es de aproximadamente $5KHz$, donde la impedancia es mínima y hay máxima transferencia de potencia, notando que la amplitud es la máxima $10V$. 

![[simulador-1 3.png]]

Si se disminuye la frecuencia a $100Hz$ la amplitud baja notablemente a $5V$ aproximadamente

![[simulador-2 2.png]]

Se aumenta la frecuencia considerablemente más que la frecuencia de resonancia, específicamente a $1MHz$. Se observa que la amplitud de la señal disminuye demasiado.  

![[simulador-3 2.png]]

Se puede concluir que efectivamente es un **filtro pasa banda**. A medida que la frecuencia de la señal se aproxima a la **frecuencia de resonancia** ($f_0 \approx 5\text{ kHz}$), la reactancia inductiva y capacitiva se compensan, permitiendo la máxima transferencia de amplitud hacia la carga ($10V$). 

Por el contrario, cuando la frecuencia se aleja de la frecuencia de resonancia, ya sea disminuyendo o aumentando la frecuencia, la impedancia del circuito del conjunto $LC$ aumenta drásticamente, provocando una caída notable en la tensión de salida. Esto demuestra que el filtro posee una **selectividad** definida, permitiendo el paso únicamente de un rango de frecuencias ($BW$) alrededor de su centro, y rechazando tanto las bajas como las altas frecuencias. 
 
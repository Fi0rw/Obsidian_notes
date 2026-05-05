## Conceptos Fundamentales
##### Reactancia
La reactancia es **la "resistencia" que ofrecen los compontenes $C$ (Capacitor/Condensador) y $L$ (Inductor/Bobina) al paso de la corriente alterna ($CA$)**. Se diferencia de la resistencia común ($R$) en que **depende de la frecuencia**. 

> Una resistencia $R$ siempre opone el mismo valor sin importar la frecuencia. Una reactancia **cambia** según la frecuencia de la señal. Se mide en $\text{Ohm }(\Omega)$ igual que la resistencia. 

Hay dos tipos: 
- **Reactancia Capacitiva** ($X_{c}$), del capacitor
- **Reactancia Inductiva** ($X_{l}$), del inductor

##### Reactancia Capacitiva ($X_{c}$)
$f$ la frecuencia de la señal ($Hz$), $C$ la capacitancia del condensador $(F)$ y $\pi ≈3.1416$. Cuando la frecuencia sube, la reactancia es más baja. Por lo tanto, a menor oposición más corriente pasa. Por otro lado, cuando la frecuencia baja, mayor es la reactancia y por lo tanto mayor oposición hay sobre la corriente (pasa menos).  
$$X_{c} = \frac{1}{2\pi fC}$$
> **A tener en cuenta**: El condensador en serie deja pasar frecuencias ALTAS, bloquea las BAJAS. 


##### Reactancia Inductiva ($X_{l}$)
$f$ la frecuencia de la señal ($Hz$), $L$ la inductancia del inductor $(H)$ y $\pi ≈3.1416$. Cuando la frecuencia sube, mayor oposición hay a la corriente (menos corriente pasa). Por otro lado, cuando la frecuencia baja, menor oposición hay a la corriente (más corriente pasa).  
$$X_{l} = 2\pi fL$$
> **A tener en cuenta**: El inductor en serie deja pasar frecuencias BAJAS, bloquea las altas.


##### Impedancia ($Z$)
La impedancia es **la resistencia total que un circuito RLC serie (con resistencia, condensador y bobina) ofrece al paso de la corriente alterna ($CA$)**. Si la impedancia ($Z$) es alta, hay menos corriente y menos potencia en $R_{x}$. Por otro lado, si la impedancia ($Z$) es baja, hay más corriente y más potencia en $R_{x}$. 
$$Z = \sqrt{R²+(X_{l}-X_{c}​)²​}$$
El caso especial es cuando $X_{l} = X_{c}$, la parte dentro de la raíz es cero. Esto ocurre a la **frecuencia de resonancia** (donde $Z$ es mínima). 
$$Z = \sqrt{ R² + (X_{c}- X_{l})² } = \sqrt{ R² + 0 } = R$$

##### Frecuencia de Resonancia ($f_{o}$)
La frecuencia de resonancia es el punto exacto en el que un sistema tiende a oscilar con su máxima amplitud porque la oposición interna se cancela o se minimiza. 
$$ f_{0} = \frac{1}{2\pi \sqrt{ LC }}$$
Consecuencias de la resonancia: 
- **Impedancia mínima ($Z$)**: Como las reactancias se restan y dan cero, la única oposición al paso de la corriente es la resistencia real ($R$) lo que permite que la corriente sea máxima.
- **Corriente máxima**: Al haber la menor resistencia posible, la corriente ($I$) alcanza su pico más alto. 
- **Transferencia de Potencia**: Es el punto donde el circuito aprovecha al máximo la energía de la fuente.

 ---

## Transmisor y Receptor 
- $T_{x}$ ($G$): **Generador/Transmisor de señal** = la fuente de señal eléctrica 
- $R_{x}$ ($R$)**: **Receptor/Carga** = el componente que "recibe" y consume la potencia de la señal
La **Potencia ($P$)** es la cantidad de energía que el transmisor ($T_x$) logra entregar efectivamente al receptor ($R_{x}$) en un tiempo determinado. Se mide en Watts ($W$) y se calcula multiplicando la intensidad de la corriente elevado ($I²$) al cuadrado por la resistencia del receptor ($R_{x}$)  
$$P = I² \times R_{x}$$
Básicamente el objetivo es que el **receptor ($R_{x}$)** reciba la información con la mayor potencia posible para poder entenderla. El obstáculo de esto sería la **impedancia ($Z$)** que es la resistencia total que intenta frenar esa energía. Para poder hacerlo, se utilizan los **filtros ($RC$, $RL$ o $RLC$)**, que permiten manipular esa impedancia según la frecuencia de la señal. Aquí entra la **resonancia ($f_{0}$)**, al combinar $L$ y $C$ en un filtro pasabanda ($RLC$) se logra que en una frecuencia específica la impedancia sea mínima, dejando pasar toda la potencia del transmisor ($T_{x}$). 

---

## Filtro
Un **filtro** es un circuito que, aprovechando las propiedades de $C$ y $L$, **selecciona qué frecuencias pasan hacia el receptor y cuáles son bloqueadas y atenuadas**. 
- Son fundamentales en Comunicaciones, Audio, Telecomunicaciones y Redes de Computadoras, Electrónica de potencia. 

| Filtro         | ¿Qué deja pasar?                         | ¿Qué bloquea?     |
| -------------- | ---------------------------------------- | ----------------- |
| **Pasa Altos** | Frecuencias altas (f > fci)              | Frecuencias bajas |
| **Pasa Bajos** | Frecuencias bajas (f < fcs)              | Frecuencias altas |
| **Pasabanda**  | Una banda de frecuencias (fci < f < fcs) | Todo lo de afuera |

--- 

## Filtro Pasa Altos
El condensador está **en serie** entre el generador ($CA$) y el receptor ($R_{x}$), cuando se encuentra en serie deja pasar altas frecuencias. Por otro lado si se encontrara en paralelo dejaría pasar las bajas.  

![FILTRO PASA ALTO C EN SERIE](https://www.learningaboutelectronics.com/imagenes/Filtro-RC-paso-alto.png)

![FILTRO PASA ALTOS](https://www.learningaboutelectronics.com/imagenes/Filtro-RL-paso-alto.png)

Con la formula $X_{c} = \frac{1}{2\pi fC}$ es posible concluir:
- Si $f$ (frecuencia) es alta, $X_{c}$ es baja (la reactancia capacitiva es baja), por lo tanto bajará la oposición a la corriente que circula por el circuito (sube la corriente) permitiendo mayor potencia en $R_{x}$  
- Si $f$ (frecuencia) es baja, $X_{c}$ es alta (la reactancia capacitiva es alta), por lo tanto aumentará la oposición a la corriente en el circuito (baja la corriente) y por ende la potencia en $R_{x}$ baja

~~~
Potencia
en Rx  │              ___________
       │            /
       │           /
       │          /
       │_________/
       └──────────────────────── f
                 ↑
                Fci
           (frecuencia de 
            corte inferior)
~~~

**fci (Frecuencia de Corte Inferior):** Es el límite. Por encima de fci, el receptor recibe buena potencia. Por debajo de fci, la señal queda bloqueada/atenuada. Se calcula en este caso: $f_{c} = \frac{1}{2\pi RC}$

El filtro pasa altos sirve para separar ruido de baja frecuencia de una señal útil de alta frecuencia, entre otras cosas. 

--- 

## Filtro Pasa Bajos
En el filtro pasa bajos el **inductor está en serie** entre el generador ($CA$) y el receptor ($R_{x}$), cuando se encuentra en serie deja pasar las bajas frecuencias. Por otro lado, si se encontrara en paralelo dejaría pasar las altas.  

![IMAGEN FILTRO PASA BAJOS](https://www.learningaboutelectronics.com/imagenes/Filtro-RL-paso-bajo.png)

![FILTRO PASA BAJOS RC](https://www.learningaboutelectronics.com/imagenes/Filtro-RC-paso-bajo.png)

Con la formula: $X_{l} = 2\pi fL$ es posible concluir: 
- Si $f$ (frecuencia) fuera alta, $X_{l}$ (la reactancia inductiva) sería alta, por lo tanto la corriente que circula por el circuito será menor (debido a mayor oposición al paso de la corriente) y la potencia en $R_{x}$ a su vez también baja.  
- Si $f$ (frecuencia) fuera baja, $X_{l}$ (la reactancia inductiva) sería baja, por lo tanto la corriente que circula por el circito será mayor (debido a la poca oposición al paso de la corriente) y la potencia en $R_{x}$ a su vez también sera mayor.  

~~~txt
Potencia
en Rx  │___________
       │            \
       │             \
       │              \
       │               \______
       └──────────────────────── f
                       ↑
                      Fcs
                (frecuencia de 
                 corte superior)
~~~

**fcs (Frecuencia de Corte Superior):** Es el límite. Por debajo de fcs, el receptor recibe buena potencia. Por encima de fcs, la señal queda bloqueada/atenuada. En este caso se calcula: $f_{c} = \frac{R}{2\pi L}$

El filtro pasa bajos tiene como aplicación eliminar el ruido de altas frecuencias (interferencias) de una señal, entre otras cosas. 

--- 

## Filtro Pasa Banda - RLC Serie
Debido a que el inductor y el condensador tienen efectos opuestos y ambos **están en serie**:
- **Frecuencias muy bajas**: $X_{l}$ baja (la reactancia inductiva baja). $X_{c}$ sube (la reactancia capacitiva sube). Por lo tanto, $Z$ (la impedancia) total sería alta y habría **poca potencia en $R_{x}$** 
- **Frecuencias muy altas**: $X_{l}$ sube (la reactancia inductiva sube). $X_{c}$ baja (la reactancia capacitiva baja). La $Z$ (impedancia) total sería alta y habría **poca potencia en $R_{x}$**. 
- **Frecuencia de Resonancia** ($f_{0}$): En la frecuencia de resonancia $X_{l}$ y $X_{c}$ se cancelan entre si y la impedancia ($Z$) cae enteramente en en $R_{x}$ por lo tanto **la potencia en $R_{x}$ es máxima**. 

![FILTRO PASA BANDA RLC SERIE](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fimg.freepik.com%2Fvector-premium%2Ffiltro-paso-banda-serie-rlc_786898-205.jpg%3Fw%3D2000&f=1&nofb=1&ipt=081e8824d093da8ea85e953a4fab5b386bf11b91ae3dea48a9ab370e001c8689)

~~~
Potencia
en Rx  │            Pico (fo)
       │           /\
       │          /  \
       │         /    \
       │        /      \
       │_______/        \______
       └──────────────────────── f
               ↑   ↑   ↑
              fci  fo  fcs
         
         |←── Ancho de Banda ──→|
              AB = fcs - fci
~~~

- **BW (Band Width)** = El ancho de banda es el rango de frecuencias dentro del cual **la mayor parte de la potencia de la señal se transfiere al receptor** $BW = f_{cs} - f_{ci}$ 
	- **BW angosto** → el filtro es muy "selectivo" (deja pasar pocas frecuencias)
	- **BW ancho** → el filtro es poco selectivo (deja pasar muchas frecuencias)
- **fo** = frecuencia de resonancia = frecuencia central = pico de potencia $f_{0}  = \frac{1}{2\pi \sqrt{ LC }}$
- **fci** = frecuencia de corte inferior $f_{ci} = f_{0} - \frac{BW}{2}$
- **fcs** = frecuencia de corte superior $f_{cs} = f_{0} + \frac{BW}{2}$
- La línea de **-3 dB** marca los puntos fci y fcs, el punto donde la potencia cae a la **mitad** del valor máximo $−3 dB≈ \frac{Pmax}{2}​​$ En términos de **voltaje** (que es lo que muestra el osciloscopio), -3 dB corresponde a que la amplitud cae al **70.7%** del máximo $V_{corte}​=V_{max} ​\times 0.707$

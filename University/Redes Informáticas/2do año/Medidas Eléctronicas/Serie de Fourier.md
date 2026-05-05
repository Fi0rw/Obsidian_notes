## Conceptos Fundamentales
##### Señal Periódica
Una **señal** en eléctronica es simplemente un voltaje que varía con el tiempo. Cuando esa variación **se repite** de forma regular, se llama **señal periódica**. 

El parámetro que describe esa repetición es el **periodo** ($T$): es el tiempo que tarda en completar un ciclo completo. La **frecuencia** ($f$) es cuántos ciclos por segundo se completan. La **unidad de la frecuencia** se mide en **Hertz** ($Hz$). E.g: 
- $1Hz$ sería $1$ ciclo por segundo.
- La voz humana tiene frecuencias entre $80Hz$ y $1100Hz$ 
- Un teléfono WiFi trabaja en $2.4GHz$ ($2.400.000.000Hz$) 

$$f = \frac{1}{T}$$
$$T = \frac{1}{f}$$


##### Onda Senoidal
La onda senoidal es la señal periódica más básica y fundamental que existe. Es la **única forma de onda que no cambia su forma al pasar por circuitos lineales** (como capacitores, resistencias e inductores). Solo puede cambiar su amplitud y su fase, pero sigue siendo senoidal. Su forma matemática es: 
- $A$ = amplitud (qué tan alta es la onda, en $V$)
- $f$ = frecuencia (qué tan rápido oscila, en $Hz$)
- $t$ = tiempo
- $φ$ = fase (en qué punto del ciclo comienza la onda) 

$$ v(t) = A⋅\sin (2\pi ft  + φ)$$

![[onda-senoidal.png]]

--- 

## Serie de Fourier
En presencia de una señal complicada (como la forma de onda de la voz al hablar) la señal parece caótica, sube y baja de forma aparentemente irregular. 

_Fourier_ dice: "Esa señal complicada es exactamente igual a la suma de muchisimas senoidales simples, cada una con su propia frecuencia, amplitud y fase"

##### Definición Formal
Para cualquier señal periódica $f(t)$ con periódica $T$, la Serie de Fourier es: 

$$f(t) = \frac{a_{0}}{2} + \sum_{n=1}^{\infty} [a_n \cos(\frac{2\pi nt}{T}) + b_n \sin(\frac{2\pi nt}{T})]$$
1. $f(t)$ **la señal total**: Es la función que se quiere representar (la voz, una onda cuadrada, o una señal de datos), depende del tiempo ($t$), lo que significa que su valor cambia a medida que el reloj avanza.

2. $\frac{a_{0}}{2}$ **componente de continua**: Es el valor promedio de la señal en un ciclo completo. Físicamente, representa un desplazamiento vertical si la señal está centrada en 0V, este término vale 0. 

3. $\sum_{n=1}^{\infty}$ **sumatoria**: Indica que se debe **sumar** una cantidad infinita de términos. 

4. $n$ **el número de armónicas**: Es un contador de números enteros. $n = 1$ es la frecuencia fundamental, que define el ritmo principal de la señal. $n > 1$ son armónicas, que vibran más rápido y se encargan de darle la "forma" final a la onda. 

5. $a_{n}$ y $b_{n}$ **los coeficientes**: Representan la amplitud de cada armónica. Determinan qué tan fuerte suena o influye cada pieza en la suma total. 

6. $\cos(\dots)$ y $\sin(\dots)$ **las ondas base**: Son las funciones trigonométricas que generan las ondas suaves y repetitivas
	- **el argumento** $\frac{2\pi nt}{T}$:
		- $T$ es el **periódo**, el tiempo que tarda la señal original en dar una vuelta completa. 
		- $\frac{2\pi}{T}$ es la **frecuencia angular fundamental** ($ω_{0}$)
		- al multiplicar por $n$ se logra que cada armónica sea exactamente $n$ veces más rápida que la anterior. 

---

## Simulador Serie de Fourier
Descomposición de una **onda cuadrada** onda cuadrada de amplitud $A$ tiene la Serie de Fourier: 

$$f(t) = \frac{4A}{\pi}(\sin(2\pi t) + \frac{\sin(3ωt)}{3} + \frac{\sin(5ωt)}{5} + \frac{\sin(7ωt)}{7}+ \dots)$$
Donde $ω = 2πf$ es la frecuencia angular de la fundamental. Ademas, solo aparecen armónicas **impares** ($1,3,5,7,9\dots$) debido a que las **pares** tienen coeficiente cero, no contribuyen. La amplitud de cada armónica decrece como $\frac{1}{n}$

![[fourier-0.png]]

En primera instancia se ve **solo la fundamental**: Frecuencia de $0.3Hz$ y una Amplitud de $9V$. Se observa una senoidal suave y perfecta. Nada inusual. 

![[fourier-1.png]]

Al sumarle la **3ra armónica** a la fundamental las partes superiores e inferiores de la onda empiezan a deformarse, se "achatan" un poco. La onda deja de ser perfectamente redonda, va tomando forma cuadrada. **Fundamental + 3ra armónica** ($0.9 Hz$, amplitud $3V$)
- Matemáticamente se calcula: $9\sin(0.6\pi t) + 3\sin(1.8\pi t)$

![[fourier-2.png]]

Al sumarle la **5ta armónica** el achatamiento es más pronunciado. Las esquinas empiezan a aparecer. La forma comienza a parecerse más a una onda cuadrada aunque todavía con bordes redondeados. **Fundamental + 3ra + 5ta** ($1.5 Hz$, amplitud $1.8V$)

![[fourier-4.png]]

Al sumarle la **7ma armónica** la onda cuadrada es bastante reconocible. Las esquinas son más agudas, las partes planas son más planas. **Fundamental + 3ra + 5ta + 7ma** ($2.1 Hz$, amplitud ≈ $1.28V$) 

![[fourier-5.png]]

Con **infinitas armónicas impares** se obtendría una onda cuadrada perfecta con esquinas afiladas y lados completamente planos.  

![ONDA CUADRADA PERFECTA](https://www.researchgate.net/profile/David-Martinez-Zorrilla/publication/260403799/figure/fig7/AS:669566984282135@1536648617411/Grafico-de-la-onda-cuadrada.png)

---

## Espectro de Frecuencias
La Serie de Fourier permite representar cualquier señal de dos formas equivalentes: 

1. **Dominio del tiempo**: el gráfico clásico de voltaje vs tiempo que muestra el osciloscopio. Se observa cómo sube y baja la señal segundo a segundo. 
2. **Dominio de la frecuencia**: un gráfico de amplitud vs frecuencia que muestra el espectro. Se observa qué frecuencias están presentes y con qué fuerza. 

Ambas representaciones contienen exactamente la misma información, solo la muestran desde perspectivas distintas. El espectro de frecuencia en la siguiente imagén: Cada barra representa una armónica, esto se llama **espectro de líneas** y es la "firma" de la onda cuadrada.

![DOMINIO DEL TIEMPO Y DOMINIO DE LA FRECUENCIA](https://portalvasco.com/blog/ficheros/tecnica/espureas02.jpg)

---

## Ancho de Banda y Armónicas
Si se quiere transmitir una onda cuadrada "razonablemente bien", se necesita transmitir NO solo la fundamental, sino también sus primeras armónicas. Cuantas más armónicas se transmiten, más fiel es la reproducción de la onda. 

Por lo cual, se necesita un **ancho de banda** (rango de frecuencias) proporcional a la calidad que se quiere. Cada sistema de comunicación tiene un ancho de banda limitado, y eso determina qué tipo de señal se puede transmitir fielmente. E.g:
- **Línea telefónica analógica**: ancho de banda de $300Hz$ a $3400Hz$. Solo transmite las frecuencias de la voz humana necesarias para la inteligibilidad, no la señal completa. 
- **Audio CD**: va de $20Hz$ a $20KHz$ cubriendo todo el rango audible humano. 
- **WiFi**: usa gigahertz de ancho de banda para transmitir una cantidad considerable de datos por segundo. 

--- 

## Filtros y Armónicas
Un filtro es un circuito que modifica selectivamente la amplitud (y fase) de distintas componentes frecuenciales de una señal. No "bloquea" señales, lo que hace es **atenuar o dejar pasar armónicas específicas**. 

##### Pasa Bajos
El filtro pasa bajos es un circuito que permite el paso de las bajas frecuencias y las altas presentan atenuación. Utiliza un inductor en serie y un condensador en derivación. ([[Filtros Repaso#Filtro Pasa Bajos]])

En un pasa bajos con un inductor ($L$) = $1mH$ y el condensador ($C$) = $1µF$, la señal empieza a atenuarse por encima de aproximadamente $1KHz$. Si se transmite una onda cuadrada de $100Hz$: 
- **Fundamental**: 100 Hz :LiArrowBigRight: pasa con amplitud completa
- **3ra armónica**: 300 Hz :LiArrowBigRight: pasa con buena amplitud
- **5ta armónica**: 500 Hz :LiArrowBigRight: pasa con amplitud reducida
- **7ma armónica**: 700 Hz :LiArrowBigRight: pasa pero atenuada
- **9na armónica**: 900 Hz :LiArrowBigRight: casi bloqueada
- **11va armónica**: 1100 Hz :LiArrowBigRight: bloqueada
- **13va armónica**: 1300 Hz :LiArrowBigRight: bloqueada
- ...y todo lo demás bloqueado

La onda cuadrada sale del filtro **redondeada**. Perdió sus esquinas porque las esquinas afiladas requieren armónicas altas (alta frecuencia) que el filtro bloqueó.

Esto tiene aplicaciones prácticas enormes. E.g: en audio digital, cuando se digitalizá música, se crea una señal con armónicas muy altas (el ruido de cuantización). Antes de enviar esa señal al parlante, se pone un filtro pasa bajos para eliminar esas armónicas artificiales y que el sonido sea suave.

##### Pasa Altos
Si se toma esa misma onda cuadrada y se aplica a un filtro pasa altos ([[Filtros Repaso#Filtro Pasa Altos]]):
- Fundamental: 100 Hz :LiArrowBigRight: bloqueada
- 3ra armónica: 300 Hz :LiArrowBigRight: bloqueada
- 5ta armónica: 500 Hz :LiArrowBigRight: parcialmente bloqueada
- Armónicas altas: :LiArrowBigRight: pasan

Queda solo con las armónicas de alta frecuencia. La señal de salida son básicamente **pulsos muy angostos** en los momentos donde la onda cuadrada cambia de nivel (las "transiciones"). Esto se usa en electrónica para **detectar bordes** de señales digitales.

##### Pasa Banda
Si se transmite una onda compleja que tiene componentes en muchas frecuencias a un filtro pasa banda ([[Filtros Repaso#Filtro Pasa Banda - RLC Serie]]) el filtro deja pasar solo las armónicas cercanas a la frecuencia de resonancia ($f_{0}$).

Aplicación directa: la **radio AM y FM**. Cada estación de radio transmite en una frecuencia central diferente. Cuando sintonizás una estación, el receptor aplica un filtro pasa banda centrado en esa frecuencia. Solo pasan las señales de esa emisora, y las demás quedan bloqueadas. La "sintonía" es literalmente ajustar la frecuencia central del filtro pasa banda.


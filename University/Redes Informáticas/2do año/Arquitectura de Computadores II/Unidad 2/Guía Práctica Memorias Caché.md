## Dirección de Memoria
La memoria es un edificio con muchos departamentos. Cada departamento guarda exactamente **una unidad de dato** (una palabra, un byte o lo que sea). Para encontrar cualquier departamento se necesita su **número de puerta**, esa seria la **dirección de memoria**. 

Si el edificio tiene **8 departamentos**, serían del **0 al 7**. Para representar el número 7 en binario se necesitan **3 bits** (debido a que $2³ = 8$) 

> Si se tienen $N \text{ posiciones de memoria}$, se necesitan $\log_{2}(N) \text{ bits}$ para la dirección. 

| Posiciones | Bits necesarios      |
| ---------- | -------------------- |
| 2          | 1 bit (2¹ = 2)       |
| 8          | 3 bits (2³ = 8)      |
| 256        | 8 bits (2⁸ = 256)    |
| 1024 (1K)  | 10 bits (2¹⁰ = 1024) |
| 1M         | 20 bits (2²⁰)        |
| 256M       | 28 bits (2²⁸)        |

## MAPEO DIRECTO - Palabra, Linea y Etiqueta
Cuando la CPU quiere un dato, genera una dirección. Esa dirección hay que interpretarla para saber: 
1. ¿En qué bloque de la memoria principal está el dato? 
2. ¿En qué línea de la cache debería estar ese bloque? 
3. ¿En qué posición dentro del bloque está exactamente el dato? 
Para responder a ello, la dirección se divide en 3 partes y cada parte responde a una pregunta distinta: 

~~~txt
┌─────────────┬──────────────┬──────────┐
│   ETIQUETA  │     LÍNEA    │  PALABRA │
└─────────────┴──────────────┴──────────┘
~~~

##### Bits de PALABRA
Responde a la pregunta de **¿Cuál es el byte/palabra exacto dentro del bloque?**.Un bloque/línea tiene varios datos adentro. Si la línea tiene $256$ palabras, se necesita un número del $0$ al $255$ para identificar cuál quiero. Para representar $256$ opciones se necesitan $2⁸ = 8bits$

$$\text{Bits de PALABRA} = 2^{\text{cantidad de palabras por línea}}$$

##### Bits de LÍNEA o CONJUNTO
Responde a la pregunta **¿en cuál de todas las lineas de caché está el bloque?**. Si la caché tiene **4096 líneas**, necesito un número del $0$ al $4095$ para identificarlas. $2¹²$ = $12 bits$.

$$\text{Bits de LÍNEA} = 2^\text{cantidad de líneas en la caché}$$
Para encontrar cuántas líneas hay en la caché: 

$$\text{TOT. LÍNEAS} = \text{TAM. MC / TAM. LÍNEA}$$
Por ejemplo: Caché de $1M$ palabras, líneas de $256$ palabras, $1M/256$ = $4096$ Líneas = $12bits$ de LÍNEA. 

##### Bits de ETIQUETA
Responde a la pregunta **¿de cuál de los muchos bloques posibles que compiten por esa línea es el que está cargando ahora?**. Como hay muchos bloques de memoria que líneas de caché, una misma línea pueden corresponder muchos bloques distintos. La etiqueta identifica cuál de ellos está actualmente en la caché. 

$$\text{ Bits de ETIQUETA} = \text{TOT. de Bits} - \text{Bits de LÍNEA} - \text{Bits de PALABRA}$$

---

## MAPEO ASOCIATIVO - Etiqueta y Palabra
**Desaparece el campo de LÍNEA**. En el mapeo asociativo un bloque puede ir a **cualquier línea**. No hay una línea fija asignada, así que no tiene sentido codificar un número de línea en la dirección. En cambio, la etiqueta tiene que ser más grande para identificar unívocamente un bloque completo. 

$$\text{Bits de ETIQUETA = TOT. MEMORIA} - \text{BITS PALABRA}$$

~~~txt
┌──────────────────────────┬──────────┐
│        ETIQUETA          │  PALABRA │
│         20 bits          │   8 bits │
└──────────────────────────┴──────────┘
~~~

## Mapeo ASOCIATIVO POR CONJUNTO - Etiqueta, Conjunto y Palabra
Es la combinación de las dos anteriores y la más usada en la práctica. Resuelve los problemas de ambas. 

Su estructura es la siguiente: la caché se divide en **v conjuntos**, donde cada conjunto tiene **k líneas**. La relación es: 

$$TOT. LÍNEAS = CONJUNTOS \times \text{LÍNEAS POR CONJUNTO}$$
La dirección se divide en: 

~~~txt
┌─────────────┬────────────┬────────────┐
│   ETIQUETA  │   CONJUNTO │   PALABRA  │
└─────────────┴────────────┴────────────┘
~~~

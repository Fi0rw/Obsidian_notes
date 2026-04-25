## Memoria Caché
La CPU es extremadamente rápida. La memoria principal (RAM) es mucho más lenta. Esta diferencia de velocidad es el problema central: si cada vez que la CPU necesita un dato tiene que esperar a que la memoria principal responda, se desperdicia una enorme cantidad de tiempo. El objetivo de la caché es lograr que el sistema tenga **velocidad cercana a la de la memoria más rápida** y al mismo tiempo **capacidad cercana a la de la memoria más grande y barata**. Ese es el equilibrio que busca.

Hay una **memoria principal grande y lenta**, y una **memoria caché pequeña y rápida**. La caché guarda una **copia de partes de la memoria principal** (las partes que se están usando). 

Cuando la CPU necesita leer una palabra, el proceso es:
1. Se busca primero en la caché.
2. **Si está** (esto se llama **acierto** o _hit_): se entrega directamente al procesador. Rápido.
3. **Si no está** (esto se llama **falla** o _miss_): se va a buscar a la memoria principal, se trae el **bloque entero** que contiene esa palabra a la caché, y luego se le entrega la palabra al procesador.

> **¿Por qué se trae un bloque entero y no solo la palabra?** Por el **principio de localidad de referencias**: si el programa accedió a una posición de memoria, es muy probable que en breve acceda a posiciones cercanas (el siguiente renglón de código, el siguiente elemento de un array, etc.). Entonces conviene traer el vecindario completo de una vez.

Cuando ocurre una falla, hay dos métodos para entregar la palabra a la CPU:
- **Carga inmediata (Load through)**: la palabra buscada se manda a la CPU en cuanto se encuentra, sin esperar que el bloque completo termine de transferirse a la caché.
- **Carga diferida (Load back)**: primero se completa la transferencia del bloque a la caché, y después la CPU lee desde la caché.

> **Regla clave**: la CPU **siempre lee desde la caché**, nunca directamente desde la memoria principal. Si la palabra no estaba en la caché, primero se lleva a la caché y de ahí va a la CPU.


## Organización Física de la Caché y la Memoria Principal
Cómo se organizan físicamente la memoria caché y la memoria principal? 

**Memoria principal:**
- Tiene hasta **2ⁿ palabras** direccionables (con direcciones de n bits).
- Se divide en **bloques de K palabras** cada uno.
- El número total de bloques es **M = 2ⁿ / K**.

**Caché:**
- Tiene **C líneas** (C es mucho menor que M → esto es la notación C << M).
- Cada línea tiene capacidad para **K palabras** (mismo tamaño que un bloque de memoria principal) más una **etiqueta** de algunos bits.
- La etiqueta es la que identifica **qué bloque de memoria principal** está copiado en esa línea.
- También tiene bits auxiliares: **bit de validez** (indica si la línea tiene datos válidos) y **bit de suciedad** (indica si fue modificada y difiere de la memoria principal).

Como hay muchos más bloques de memoria que líneas de caché, **una misma línea puede alojar distintos bloques en diferentes momentos**. No hay una relación permanente de uno a uno.

## Operación de lectura - El flujo completo con hardware
Cuando el procesador genera una **dirección RA (Real Adress o Dirección Física)** para leer

~~~txt
¿El bloque que contiene RA está en caché?
        |               |
       SÍ               NO
        |               |
Entregar la      Ir a memoria principal
palabra al       Asignar una línea de caché
procesador       Cargar el bloque en esa línea
                 Entregar la palabra al procesador
~~~

Físicamente, el procesador está conectado a la caché por líneas de datos, direcciones y control. La caché a su vez está conectada a la memoria principal a través del bus del sistema con **buffers** intermedios.
- **En un acierto**: los buffers hacia el bus se inhabilitan. Toda la comunicación ocurre entre procesador y caché solamente. El bus del sistema queda libre. Muy eficiente.
- **En una falla**: la dirección se pone en el bus del sistema, se trae el dato de memoria principal a través del buffer de datos, y ese dato va tanto a la caché como al procesador.

## Elementos de diseño de la Caché
La memoria caché posee un diseño específico:

##### 1. Tamaño de la caché
El tamaño es una decisión de compromiso:
- Si es **muy pequeña**: muchas fallas, se pierde el beneficio.
- Si es **muy grande**: sube el costo (la caché usa tecnología cara) y no hay espacio físico en el chip.
El objetivo es que el tamaño sea lo mínimo necesario para que el **tiempo de acceso promedio total** se parezca al tiempo de acceso de la caché sola.

##### 2. Función de correspondencia
Dado que hay muchos más bloques de memoria que líneas de caché, ¿cómo se decide **en qué línea de caché** va a cargarse cada bloque de memoria? Existen tres técnicas: 
1. **Correspondencia Directa**: Cada bloque de memoria tiene una **única línea posible** en la cache donde puede guardarse
	- La fórmula es $i = j \text{ módulo } m$ 
		- $i = \text{número de línea de caché}$
		- $j = \text{número de bloque de memoria principal}$
		- $m = \text{número de líneas de caché}$
	- Ejemplo: si hay m=4 líneas de caché, el bloque 0 va a la línea 0, el bloque 1 a la línea 1, el bloque 4 vuelve a la línea 0, el bloque 5 a la línea 1, etc. Los bloques 0, 4, 8, 12... compiten todos por la línea 0.

	- **¿Cómo se estructura la dirección de memoria?** La dirección se divide en tres campos:
		- **ETIQUETA** (s-q bits, los más significativos): identifica cuál de los muchos bloques asignados a esa línea es el que está actualmente cargado.
		- **LÍNEA** (q bits): indica en qué línea de la caché buscar.
		- **PALABRA** (w bits): indica cuál palabra dentro del bloque.

	- **Proceso de búsqueda**: 
		- Se extrae el campo LÍNEA para ir directamente a esa línea.
		- Se verifica el **bit de validez**: si es 0, hay falla (la línea no tiene nada útil).
		- Si el bit de validez es 1, se compara la ETIQUETA de la dirección con la etiqueta almacenada en esa línea.
		- Si coinciden → **acierto**, se devuelve la PALABRA indicada.
		- Si no coinciden → **falla**, se trae el bloque de memoria principal.

	- **Ventaja**: muy simple y rápida de implementar (circuitería mínima).
	- **Desventaja grave**: si el programa alterna entre dos posiciones de memoria que caen en la misma línea de caché, ambas se van expulsando mutuamente constantemente. La tasa de aciertos se derrumba. Esto se llama **thrashing** (paliza).

2. **Correspondencia Asociativa**: Un bloque de memoria puede cargarse en **cualquier línea** de la caché, sin restricción.
	- La dirección se divide en solo dos campos:
		- **ETIQUETA** (s bits): el número de bloque completo de memoria principal.
		- **RÓTULO-PALABRA** (w bits): posición dentro del bloque.
	- Para saber si un bloque está en caché, hay que **comparar la etiqueta de la dirección con las etiquetas de TODAS las líneas simultáneamente** (búsqueda en paralelo). Esto requiere hardware de comparación para cada línea.
	
	- **Ventaja**: máxima flexibilidad. No existe el problema del thrashing. Cualquier bloque puede ir a cualquier línea.
	- **Desventaja**: circuitería muy compleja y costosa. A más líneas de caché, más comparadores en paralelo necesarios.

3. **Correspondencia Asociativa por Conjuntos**: la caché se divide en **$v \text{ conjuntos}$**, donde cada conjunto tiene $k \text{ líneas}$. Es la combinación de las dos anteriores y la más usada en la práctica. Resuelve los problemas de ambas.
	- La relación es: $m = v × k \text{ (total de líneas = conjuntos × líneas por conjunto)}$
	- La asignación funciona así:
		- **Para elegir el conjunto**: se usa correspondencia directa → **i = j módulo v**
		- **Dentro del conjunto**: se usa correspondencia asociativa (el bloque puede ir a cualquiera de las k líneas del conjunto)
	- La dirección se divide en:
		- **ETIQUETA** (s-q bits)
		- **CONJUNTO** (q bits): indica a qué conjunto corresponde
		- **PALABRA** (w bits)

| Caso         | v                     | k                | Equivale a                      |
| ------------ | --------------------- | ---------------- | ------------------------------- |
| v = m, k = 1 | Un conjunto por línea | 1 línea/conjunto | Correspondencia directa pura    |
| v = 1, k = m | Un único conjunto     | Todas las líneas | Correspondencia asociativa pura |

##### 3. Algoritmos de sustitución
Cuando la caché está llena y hay una falla, hay que **elegir qué línea existente reemplazar** para cargar el nuevo bloque. En correspondencia directa no hay elección (hay solo una opción). En los esquemas asociativos hay que decidir. Los algoritmos se implementan en hardware para ser rápidos:

**LRU — Menos Recientemente Usado (Least Recently Used)** Cada línea lleva un registro de cuándo fue accedida por última vez. Se descarta la que hace más tiempo que no se usa. Es el más efectivo en la práctica porque se basa en la localidad temporal: lo que no se usó recientemente probablemente no se use pronto.

**LFU — Menos Frecuentemente Usado (Least Frequently Used)** Cada línea lleva un contador de cuántas veces fue accedida. Se descarta la de menor frecuencia de uso. Puede ser problemático si un bloque fue muy usado al inicio pero ya no se necesita.

**FIFO — Primero en entrar, primero en salir (First In First Out)** Se reemplazan las líneas en orden de llegada: la que entró primero es la primera en salir. Simple de implementar pero no necesariamente eficiente.

**Aleatorio** Se elige una línea al azar para reemplazar. Sorprendentemente, en la práctica da resultados similares al LRU y es muy fácil de implementar en hardware.


##### 4. Política de escritura
¿qué pasa cuando la CPU **escribe** un dato? La caché tiene una copia del bloque y la memoria principal tiene otra. Hay que decidir cómo mantenerlas sincronizadas. La situación depende de si el dato que se quiere escribir **ya está en la caché o no**:

| ¿Está en caché? | Política                           | Nombre técnico      | Descripción                                                                                                                                                                                                                                      |
| --------------- | ---------------------------------- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| SÍ              | Escritura Inmediata                | _Write through_     | Se escribe simultáneamente en caché y en memoria principal. Siempre están sincronizadas. Genera más tráfico en el bus.                                                                                                                           |
| SÍ              | Postescritura (Escritura Diferida) | _Write back_        | Se escribe solo en la caché. La escritura en memoria principal se difiere hasta que esa línea sea reemplazada. Más eficiente pero puede haber inconsistencias temporales. Se usa el **bit de suciedad** para marcar que la línea fue modificada. |
| NO              | Escritura Con Asignación           | _Write allocate_    | Se trae el bloque a la caché y se lo actualiza ahí.                                                                                                                                                                                              |
| NO              | Escritura Sin Asignación           | _Write no allocate_ | Se escribe directamente en memoria principal sin traer el bloque a la caché.                                                                                                                                                                     |

En sistemas con múltiples procesadores (cada uno con su propia caché) que comparten una memoria principal, surge el problema de **coherencia**: si el procesador A modifica un dato en su caché, el procesador B puede tener una copia obsoleta del mismo dato en su caché. Las soluciones son:

**Vigilancia del bus con escritura inmediata**: cada controlador de caché monitorea el bus. Si detecta que alguien escribió en una posición que también tiene en su caché, invalida su propia copia.

**Transparencia hardware**: hardware dedicado que garantiza que toda escritura en cualquier caché se propaga automáticamente a todas las demás cachés y a la memoria principal.

**Memoria excluida de caché**: las zonas de memoria compartida entre procesadores se marcan como "no cacheable". Todo acceso a esas zonas va directo a memoria principal. Sencillo pero reduce la eficiencia.


##### 5. Tamaño de línea
¿Cuántas palabras debe tener cada línea/bloque? La relación entre tamaño de bloque y tasa de acierto tiene forma de campana. 
- **Bloque pequeño**: cuando se carga un bloque, se trae poca información del vecindario. Muchas fallas.
- **A medida que crece el bloque**: se aprovecha mejor la localidad espacial, hay más datos útiles en caché. La tasa de aciertos **sube**.
- **Bloque muy grande**: empieza a bajar la tasa de aciertos. ¿Por qué?
    - Hay **menos bloques en la caché** (el mismo espacio dividido en bloques más grandes da menos bloques). Más probabilidad de que el bloque que necesitas no esté.
    - Las palabras al final del bloque están **lejos de la palabra original** y es menos probable que se necesiten pronto.
    - Cada falla trae una transferencia más grande, lo que tarda más y satura el bus.

##### 6. Número de cachés
**Cachés multinivel:** Los procesadores modernos tienen varios niveles de caché:

![](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fhardzone.es%2Fapp%2Fuploads-hardzone.es%2F2020%2F01%2FNiveles-de-cache.jpg&f=1&nofb=1&ipt=5d42fe54bce25be1ab4476b30b4ebc72235513acbd3911a358387229a42dcc69)

- **L1**: la más pequeña y rápida, integrada directamente en el chip de la CPU. Se accede en pocos ciclos de reloj.
- **L2**: más grande y algo más lenta, también suele estar en el mismo chip hoy en día.
- **L3**: aún más grande, puede ser compartida entre varios núcleos.

~~~txt
La jerarquía es: CPU → L1 → L2 → L3 → Memoria Principal → Disco.
~~~

**Caché unificada vs. fragmentada:**
- **Unificada**: una sola caché para instrucciones y datos. Simple pero puede generar cuellos de botella.
- **Fragmentada (split cache)**: se divide en caché de instrucciones y caché de datos separadas. Esto es posible porque estadísticamente hay aproximadamente una escritura por cada cuatro o más lecturas, y las instrucciones solo se leen nunca se escriben. Al separarlas se pueden optimizar independientemente y se permite acceso simultáneo a instrucciones y datos.

## Generalidades - Métricas de rendimiento 
Fórmulas son las que permiten calcular el rendimiento real de la caché. 

**Tasa de aciertos (Hit Rate — HR):**
```
HR = (Nº de aciertos / Nº total de accesos a memoria) × 100
```

Expresa qué porcentaje de las veces el dato estaba en caché.

**Tasa de falla (Miss Rate — MR):**
```
MR = (Nº de fallas / Nº total de accesos a memoria) × 100
```

Nótese que HR + MR = 100%.

**Tiempo efectivo (medio) de acceso (tea):**
```
tea = (Nº aciertos × tiempo de acierto + Nº fallas × tiempo de falla) / Nº total de accesos
```

También se puede expresar como:

```
tea = HR × t_caché + MR × t_memoria_principal
```

> **Nota**: si la caché tiene una tasa de aciertos del 90%, tiempo de acceso a caché de 10 ns y tiempo de acceso a memoria principal de 100 ns, entonces: tea = 0.90 × 10 + 0.10 × 100 = 9 + 10 = **19 ns** Sin caché sería 100 ns. Con caché es 19 ns. La mejora es enorme.


## RESUMÉN ESQUEMÁTICO PARA REPASO
~~~txt
MEMORIA CACHÉ
│
├── Problema: CPU rápida, RAM lenta → necesito algo intermedio
│
├── Principio de localidad:
│   ├── Temporal: lo que se usó recientemente se usará de nuevo
│   └── Espacial: lo que está cerca de lo usado también se usará
│
├── Estructura:
│   ├── Memoria Principal: 2ⁿ palabras, M = 2ⁿ/K bloques
│   └── Caché: C líneas (C << M), cada línea = K palabras + etiqueta
│
├── Operación de lectura:
│   ├── ACIERTO → entregar desde caché (rápido, sin usar el bus)
│   └── FALLA → traer bloque de memoria principal → cargar en caché → entregar
│
├── Funciones de correspondencia:
│   ├── Directa: i = j mod m  (1 bloque → 1 línea fija)
│   │   ├── Ventaja: simple y barata
│   │   └── Desventaja: thrashing si dos bloques compiten por la misma línea
│   ├── Asociativa: bloque va a cualquier línea
│   │   ├── Ventaja: máxima flexibilidad
│   │   └── Desventaja: circuitería compleja y cara
│   └── Asociativa por conjuntos: i = j mod v (dentro del conjunto → asociativa)
│       ├── Caso extremo v=m, k=1 → directa pura
│       ├── Caso extremo v=1, k=m → asociativa pura
│       └── Caso práctico más común: k=2 (2 vías)
│
├── Algoritmos de sustitución (para asociativas):
│   ├── LRU: descarta la menos recientemente usada ← más eficiente
│   ├── LFU: descarta la menos frecuentemente usada
│   ├── FIFO: descarta la que entró primero
│   └── Aleatorio: descarta una al azar
│
├── Políticas de escritura:
│   ├── Si dato EN caché:
│   │   ├── Write through (escritura inmediata): escribe en caché Y en RAM
│   │   └── Write back (postescritura): escribe solo en caché, difiere RAM
│   └── Si dato NO EN caché:
│       ├── Write allocate: trae bloque a caché, escribe ahí
│       └── Write no allocate: escribe directo en RAM
│
├── Tamaño de línea: compromiso, bloques medianos maximizan tasa de aciertos
│
├── Número de cachés:
│   ├── Multinivel: L1 (más rápida, en chip), L2, L3
│   └── Unificada vs. Fragmentada (datos + instrucciones separados)
│
└── Métricas:
    ├── HR = (aciertos / total) × 100
    ├── MR = (fallas / total) × 100
    └── tea = HR × t_caché + MR × t_memoria
~~~
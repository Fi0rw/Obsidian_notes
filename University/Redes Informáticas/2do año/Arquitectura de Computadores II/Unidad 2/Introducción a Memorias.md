## Memorias
Las memorias almacenan **datos e instrucciones**. Se organizan internamente en unidades llamadas **registros** (no los registros de la CPU, acá "registro" es el nombre genérico de una unidad de almacenamiento dentro de la memoria). Ninguna memoria existente es a la vez rápida, barata y de gran capacidad. Por eso un computador usa una **jerarquía de subsistemas de memoria**, algunos internos y otros externos, combinando distintas tecnologías para aprovechar lo mejor de cada una.

## Características de las Memorias 
Hay ocho categorías de características que definen a cualquier memoria:
##### 1. Ubicación
Indica si la memoria está dentro o fuera del computador.

**Interna** (dentro del computador):
- Los **registros** de la CPU: la memoria más pequeña, rápida y cara. Están físicamente dentro del procesador.
- La **memoria caché**: también interna, entre la CPU y la RAM.
- La **memoria principal (RAM)**: la más conocida como "memoria interna".

**Externa** (fuera del computador): dispositivos periféricos de almacenamiento accesibles a través de controladores de E/S. Ejemplos: discos rígidos, cintas magnéticas, pendrives.

##### 2. Capacidad 
Indica cuánto puede almacenar la memoria.

- **Memorias internas**: se expresa en **bytes** (1 byte = 8 bits) o en **palabras**. Los tamaños de palabra más comunes son 8, 16 y 32 bits.
- **Memorias externas**: siempre se expresa en **bytes** (KB, MB, GB, TB).

Siempre se busca la mayor capacidad posible para tener certeza de que todo lo necesario puede almacenarse.

##### 3. Unidad de Transferencia
Es la cantidad de bits que se mueven en una sola operación de lectura/escritura. Hay que distinguir tres conceptos relacionados:

- **Palabra**: unidad natural de organización. Su tamaño suele coincidir con el número de bits usados para representar números y con la longitud de instrucciones.
- **Unidades direccionables**: no siempre la unidad que se direcciona es una palabra entera. La relación es:

> **2ᴬ = N** (donde A = longitud de la dirección en bits, N = número de posiciones direccionables)

Según el tipo de memoria:
- **Memorias internas**: la unidad de transferencia es igual al número de líneas de datos del módulo. Puede ser igual a la palabra o mayor (64, 128, 256 bits).
- **Memoria principal**: transfiere el número de bits que se leen/escriben a la vez.
- **Memoria externa**: transfiere en unidades más grandes que la palabra, llamadas **bloques** (esto conecta directo con lo de la caché).

##### 4. Métodos de Acceso
Existen cuatro métodos de acceso: 

1. **Acceso secuencial**: Para llegar a un dato hay que **recorrer todos los datos anteriores** desde el inicio, en orden. 
	- El mecanismo de lectura/escritura se desplaza físicamente desde su posición actual hasta la deseada.
	- El tiempo de acceso es **variable**: depende de qué tan lejos esté el dato que buscás.
	- Ejemplo clásico: **cintas magnéticas**.


2. **Acceso Directo (DMA — Direct Memory Access)**: Se accede primero a una **vecindad general** del dato, y luego se hace una búsqueda secuencial corta dentro de esa zona.
	- El tiempo de acceso es **variable** pero menor que el secuencial.
	- Ejemplos: **discos rígidos** (el cabezal va al cilindro correcto, luego espera el sector), tarjetas gráficas, tarjetas de sonido.
El concepto de DMA además tiene un significado específico en arquitectura: permite que un módulo de E/S lea o escriba en memoria **sin intervención del procesador**. El controlador DMA se encarga de la transferencia, liberando al procesador para hacer otras cosas. Esto es esencial para aplicaciones que mueven muchos datos (video, audio, disco).

3. **Acceso Aleatorio (RAM - Random Memory Access)**: Se puede acceder a **cualquier posición directamente**, sin recorrer las anteriores.
	- El tiempo de acceso es **constante e independiente** de la posición y de los accesos previos.
	- Cada posición tiene su propia dirección única y puede ser accedida directamente.
	- Es **volátil**: los datos se pierden al apagar el equipo.
	- Ejemplos: **memoria principal (RAM)**, caché.

4. **Acceso Asociativo**: Es un tipo especial de acceso aleatorio donde se busca un dato **por su contenido**, no por su dirección.
	- Se hace una comparación simultánea de ciertos bits de todas las palabras almacenadas, en paralelo.
	- Cada posición tiene su propio mecanismo de comparación.
	- El tiempo de acceso es **constante** (igual que el aleatorio).
	- Ejemplos: redes neuronales artificiales, sistemas de IA.

##### 5. **Prestaciones**: 
Tres métricas clave de rendimiento. 
	1. **Tiempo de acceso (latencia)**: 
		- En memorias aleatorias: tiempo desde que se presenta la dirección hasta que el dato está disponible. 
		- En otras memorias: tiempo en posicionar el mecanismo lector en la posición deseada.
	2. **Tiempo de Ciclo de Memoria**:
		- Para memorias aleatorias: tiempo de acceso **más** un tiempo adicional antes de poder iniciar otro acceso. Este extra puede ser para que las líneas de señal se estabilicen o para regenerar datos (en el caso de memorias con lectura destructiva como la DRAM).
		- Importante: depende del **bus del sistema**, no del procesador.
		- Siempre **Tiempo de Ciclo > Tiempo de Acceso**. 
	3. **Velocidad de Transferencia**: 
		- Para memorias aleatorias: es simplemente **1 / Tiempo de Ciclo**.
		- Para otras memorias (discos, cintas), se usa la fórmula: $Tₙ = Tₐ + N/R$ 
			- $Tₙ$ = tiempo total para leer/escribir N bits
			- $Tₐ$ = tiempo de acceso medio
			- $N$ = número de bits a transferir
			- $R$ = velocidad de transferencia en bits por segundo (bps)

##### 6. Soporte Físico (Dispositivo Físico): 
 El material con el que está fabricada la memoria: 
	 - **Semiconductoras**: las más comunes hoy en día. Usadas en RAM, caché, registros, ROM, SSD.
	 - **Superficie magnética**: usadas en discos rígidos (HDD) y cintas magnéticas.
	 - **Ópticas**: CDs, DVDs, Blu-ray.
	 - **Magneto-ópticas**: combinan propiedades magnéticas y ópticas (MO discs, WORM).

##### 7. Características Físicas
**Volátil**: la información **desaparece** cuando se corta la alimentación eléctrica.
- Ejemplos: RAM, caché, registros.

**No volátil**: la información **persiste** sin necesitar energía.
- Ejemplos: discos magnéticos, memorias flash (SSD, pendrive), ROM.

**Borrable**: puede modificarse durante su uso normal (como la RAM).

**No borrable (ROM — Read Only Memory)**: no puede modificarse salvo destruyendo físicamente la unidad. Solo lectura. Se usa para firmware, BIOS, etc.


##### 8. Organización
Es la disposición o estructura física de los bits para formar palabras dentro de la memoria. Es un aspecto clave de diseño especialmente en memorias de acceso aleatorio.


## Jerarquías de Memorias
Cualquier diseñador de sistemas de memoria se enfrenta a tres preguntas: **¿Cuánta capacidad?** Siempre se necesita más. **¿Qué tan rápida?** La CPU no debe esperar. **¿A qué costo?** Debe ser económicamente razonable. 

El problema es que esos tres factores se oponen entre sí:
> **A menor tiempo de acceso → mayor costo por bit.** **A mayor capacidad → menor costo por bit.** **A mayor capacidad → mayor tiempo de acceso.**

La solución a ello es la **Jerarquía de Memorias**. En lugar de elegir una sola tecnología, se combinan varias en una jerarquía. Las memorias más rápidas, pequeñas y costosas se complementan con memorias más lentas, grandes y baratas.

![Jerarquía de memorias](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fwww.monografias.com%2Ftrabajos104%2Fjerarquia-memorias%2Fimg1.png&f=1&nofb=1&ipt=f7fb17e790ed900a668990819db9db3cbac29a5b0fbe64b3fcff491f59c51cbb)

~~~txt
REGISTROS (dentro del CPU)
      ↓
    CACHÉ (L1, L2, L3)
      ↓
MEMORIA PRINCIPAL (RAM)
      ↓
ALMACENAMIENTO EXTERNO (Disco magnético, SSD)
      ↓
ALMACENAMIENTO FUERA DE LÍNEA (Cinta magnética, WORM, MO)
~~~

**La clave del éxito** de esta jerarquía es la **disminución de la frecuencia de acceso** a medida que se baja en la jerarquía. Gracias al principio de localidad, la mayoría de los accesos se resuelven en los niveles superiores (rápidos). Los niveles inferiores (lentos) se usan con mucha menos frecuencia.

## Tipos de Memoria en la Jerarquía

**Registros internos al procesador:**
- La memoria más rápida, pequeña y costosa que existe.
- Están físicamente dentro del chip de la CPU.
- Un procesador típico tiene decenas de registros (algunos tienen cientos).
- Tecnología: volátil, semiconductora.

**Caché:**
- Pequeña y rápida. Actúa como intermediaria entre la CPU y la RAM.
- **No es visible para el programador ni para el procesador**: opera de forma completamente transparente y automática en hardware.
- Tecnología: volátil, semiconductora.

**RAM (Memoria Principal):**
- La memoria principal del sistema.
- Cada posición tiene una única dirección.
- Tecnología: volátil, semiconductora.

**Dispositivos de memoria masiva:**
- Disco rígido, pendrive, SSD, etc.
- Almacenamiento persistente de datos.

**Memorias externas:**
- Las no volátiles se llaman **memorias secundarias**.
- Son visibles al programador como **ficheros y registros**, no como bytes aislados.

## RESUMÉN PARA REPASO
~~~txt
INTRODUCCIÓN A MEMORIAS
│
├── Concepto: ninguna tecnología sola es óptima → jerarquía
│
├── Características:
│   ├── Ubicación: interna (registros, caché, RAM) / externa (disco, cinta)
│   ├── Capacidad: bytes o palabras (8/16/32 bits por palabra)
│   ├── Unidad de transferencia: palabra / bloque; 2ᴬ = N
│   ├── Método de acceso:
│   │   ├── Secuencial: recorre desde el inicio (tiempo variable) → cintas
│   │   ├── Directo (DMA): va a vecindad + búsqueda corta (variable) → discos
│   │   ├── Aleatorio (RAM): directo a cualquier posición (constante) → RAM
│   │   └── Asociativo: busca por contenido, en paralelo (constante) → IA
│   ├── Prestaciones:
│   │   ├── Tiempo de acceso (latencia)
│   │   ├── Tiempo de ciclo (> tiempo de acceso)
│   │   └── Velocidad de transferencia: 1/T_ciclo  ó  Tₙ = Tₐ + N/R
│   ├── Soporte físico: semiconductor / magnético / óptico / magneto-óptico
│   ├── Características físicas:
│   │   ├── Volátil (pierde datos sin energía) vs. No volátil
│   │   └── Borrable vs. No borrable (ROM)
│   └── Organización: disposición de bits en palabras
│
└── Jerarquía:
	├── Problema: velocidad ↑ ↔ capacidad ↓ ↔ costo ↑
	├── Solución: combinar niveles, aprovechar localidad
	└── Pirámide: Registros → Caché → RAM → Disco → Cinta
~~~

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

##### Tamaño de la caché
El tamaño es una decisión de compromiso:
- Si es **muy pequeña**: muchas fallas, se pierde el beneficio.
- Si es **muy grande**: sube el costo (la caché usa tecnología cara) y no hay espacio físico en el chip.
El objetivo es que el tamaño sea lo mínimo necesario para que el **tiempo de acceso promedio total** se parezca al tiempo de acceso de la caché sola.

##### Función de correspondencia
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


## Ciclos de Captación y Ejecución
Un programa es un conjunto de instrucciones almacenadas en memoria. La función básica de un computador es ejecutarlas. El proceso más simple tiene dos etapas que se repiten continuamente: 

~~~
INICIO → [Captar instrucción] → [Ejecutar instrucción] → (repetir) → PARADA
~~~

![Ciclo de Captacion y Ejecucion](https://external-content.duckduckgo.com/iu/?u=http%3A%2F%2F3.bp.blogspot.com%2F-TyyJ-moHb0U%2FVRnEb2dtgDI%2FAAAAAAAAAAo%2FQsCKyheHvRY%2Fs1600%2F1.png&f=1&nofb=1&ipt=61efb04b2e1af38dec9c85979e0cc5217d32804b6858d347c7d21abea08065fa)

- **Ciclo de captación (fetch)**: la CPU lee la instrucción de memoria. 
- **Ciclo de ejecución**: la CPU lleva a cabo la acción que indica esa instrucción. 
El ciclo se detiene únicamente si: la máquina se apaga, ocurre un error o se ejecuta una instrucción que lo detiene. 

##### PC (Program Counter)
Al inicio de cada ciclo, la CPU necesita saber **de dónde captar la próxima instrucción**. Para esto existe el **PC (Program Counter)**. Es un registro que guarda la dirección de memoria de la siguiente instrucción a ejecutar. 

La CPU **siempre incrementa el PC automáticamente** después de captar cada instrucción, para pasar a la siguiente posición de memoria. 
- E.g: si el `PC=300`, capta la instrucción en la posición `300`, luego el `PC=301`, capta la de `301`, pasa a la `302` y así sucesivamente. 

El **PC (Program Counter)** puede alterarse con instrucciones de salto (`JMP, JNZ, JZ,` etc)

##### IR (Instruction Register)
La instrucción captada de memoria no se ejecuta "en el aire": se guarda en el **IR (Instruction Register)**. La instrucción está codificada en binario y específica la acción que debe realizar la CPU. 

Cuando la **CPU decodifica el IR**, la acción puede ser de uno de estos cuatro tipos: 
- **Procesador-Memoria**: transferir datos entre CPU y memoria (read/write).
- **Procesador-E/S**: transferir datos entre CPU y un módulo de E/S. 
- **Procesamiento de datos**: realizar una operación aritmética o lógica. 
- **Control**: alterar la secuencia de ejecución (saltar a otra dirección). 
Una instrucción puede combinar varias de estas acciones.

##### Ejemplo 
Una máquina de ejemplo con estas características: 
- Instrucciones y datos: **16 bits**
- Un solo registro de datos: el **Acumulador (AC)**
- Formato de instrucción: **4 bits de código de operación (codop) + 12 bits de dirección**
- Registros: **PC** (dirección de próxima instrucción), **IR** (instrucción en ejecución), **AC** (dato temporal)
- Instrucciones disponibles:
    - `0001` → Cargar AC desde memoria
    - `0010` → Almacenar AC en memoria
    - `0101` → Sumar a AC un dato de memoria

**El programa de ejemplo** suma el contenido de la posición 940 con el de la posición 941, y guarda el resultado en 941. Las posiciones de memoria tienen: 940 = 0003, 941 = 0002.

Las tres instrucciones en memoria son:
- Posición 300: `1940` → Cargar AC desde posición 940
- Posición 301: `5941` → Sumar a AC el valor de posición 941
- Posición 302: `2941` → Almacenar AC en posición 941

**Los 6 pasos de ejecución:**

| Paso | Ciclo     | Qué ocurre                                        |
| ---- | --------- | ------------------------------------------------- |
| 1    | Captación | PC=300, se carga IR=1940                          |
| 2    | Ejecución | IR dice "cargar AC desde 940" → AC=0003           |
| 3    | Captación | PC=301, se carga IR=5941                          |
| 4    | Ejecución | IR dice "sumar 941 a AC" → AC = 0003+0002 = 0005  |
| 5    | Captación | PC=302, se carga IR=2941                          |
| 6    | Ejecución | IR dice "guardar AC en 941" → posición 941 = 0005 |
> 💡 **Resultado final**: la posición 941 pasó de tener 0002 a tener 0005 (3+2=5). Se usaron 3 ciclos de instrucción completos (3 captaciones + 3 ejecuciones).


##### El ciclo de instrucción detallado (diagrama de estados)
El ciclo básico de dos pasos es una simplificación. En realidad, cada instrucción puede pasar por hasta 7 estados:

1. **IAC** — Cálculo de la dirección de la instrucción: determina de dónde captar la próxima instrucción (normalmente es PC + 1 o PC + 2 dependiendo del tamaño de la instrucción y si la memoria es de 16 bits o de bytes).
2. **IF** — Captación de instrucción: la CPU lee la instrucción de memoria y la mete en el IR.
3. **IOD** — Decodificación: analiza el IR para saber qué operación hacer y qué operandos usar.
4. **OAC** — Cálculo de la dirección del operando: si la instrucción requiere leer/escribir un dato en memoria o E/S, calcula su dirección.
5. **OF** — Captación del operando: lee el dato desde memoria o E/S.
6. **DO** — Operación con los datos: ejecuta la operación (suma, comparación, etc.).
7. **OS** — Almacenamiento del operando: escribe el resultado en memoria o E/S.

No todos los estados ocurren en cada instrucción. Y algunos pueden repetirse (por ejemplo, si hay varios operandos).

---

## Interrupciones
¿Por qué existen las interrupciones? Sin interrupciones, si el procesador le manda datos a una impresora, tendría que **quedarse esperando** (sin hacer nada) hasta que la impresora termine de imprimir. Esto es un enorme desperdicio, porque los dispositivos externos son **muchísimo más lentos** que el procesador (la espera podría ser miles de ciclos de instrucción).

Las **interrupciones** son el mecanismo que soluciona esto: permiten que el procesador siga ejecutando otras instrucciones mientras el dispositivo externo trabaja, y cuando ese dispositivo termina, avisa al procesador mediante una señal. Existen diferentes tipos de interrupciones: 

| Tipo                     | Descripción                                                                                                                                   |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------- |
| **De programa**          | Generadas por la ejecución de una instrucción: overflow aritmético, división por cero, instrucción inexistente, acceso a memoria no permitido |
| **De temporización**     | Generadas por un temporizador interno del procesador. Permiten al SO ejecutar tareas regulares                                                |
| **De E/S**               | Generadas por un controlador de E/S al terminar una operación o al detectar un error                                                          |
| **De fallo de hardware** | Generadas por fallos físicos: falta de alimentación, error de paridad en memoria                                                              |

¿Cómo funciona el ciclo de instrucción con interrupciones? Se agrega un tercer ciclo al ciclo de instrucción:

~~~txt
INICIO → [Captar] → [Ejecutar] → [Comprobar interrupción] → (repetir)
~~~

En el **ciclo de interrupción**, el procesador chequea si hay una señal de interrupción pendiente:
- **Si no hay**: vuelve al ciclo de captación normalmente.
- **Si hay**: el procesador hace dos cosas:
    1. **Suspende** la ejecución del programa actual y **guarda su contexto** (principalmente el valor actual del PC, para poder retomar después).
    2. **Carga en el PC** la dirección del **gestor de interrupción** (interrupt handler), que es una rutina del sistema operativo que atiende la interrupción.
Cuando el gestor termina, el procesador restaura el contexto guardado y retoma el programa original desde donde lo dejó.

**¿Hay overhead (costo extra)?** Sí. Atender una interrupción requiere ejecutar instrucciones extra (en el gestor). Pero ese costo es **mucho menor** que el tiempo que se perdería esperando sin hacer nada al dispositivo de E/S. La ganancia neta es enorme.

---

## Interrupciones Múltiples
Pueden llegar varias interrupciones al mismo tiempo o mientras se atiende una. Hay dos estrategias:

**Estrategia 1 — Desactivar interrupciones (secuencial)** Mientras se atiende una interrupción, se **inhabilitan** todas las demás. Las que llegan en ese momento quedan pendientes y se atienden después, en orden estricto. Es simple pero ignora prioridades.

**Estrategia 2 — Prioridades (anidado)** Se asigna una prioridad a cada tipo de interrupción. Una interrupción de **mayor prioridad** puede interrumpir a un gestor que está atendiendo una de **menor prioridad**. Esto permite atender eventos críticos (como datos que llegan por red y que se pueden perder si no se procesan rápido) sin importar qué otra cosa esté haciendo el procesador.

--- 

## Estructuras de Interconexión
Los tres tipos de módulos del computador (CPU, Memoria, E/S) necesitan comunicarse. El conjunto de líneas que los conecta se llama **estructura de interconexión**.

**¿Qué información circula?** Cada módulo tiene sus propias entradas y salidas:
- **Memoria**: recibe una dirección y una señal de Leer/Escribir, y entrega o recibe datos.
- **Módulo de E/S**: similar a la memoria, pero controla dispositivos externos a través de **puertos**. También puede enviar señales de interrupción a la CPU.
- **CPU**: emite direcciones y señales de control, lee/escribe datos, y recibe señales de interrupción.

Los tipos de transferencias que debe soportar la estructura son:
- Memoria → Procesador (leer instrucción o dato)
- Procesador → Memoria (escribir dato)
- E/S → Procesador (leer dato de dispositivo)
- Procesador → E/S (enviar dato a dispositivo)
- Memoria ↔ E/S (con DMA, sin pasar por el procesador — se verá más adelante)

--- 

## Interconexión con Buses
**¿Qué es un bus?** Un **bus** es un camino de comunicación **compartido** entre dos o más dispositivos. La clave es que es compartido: todos los dispositivos conectados pueden "escuchar" las señales, pero **solo uno puede transmitir a la vez** (si dos transmiten simultáneamente, las señales se distorsionan).

Un bus está formado por varias líneas paralelas, cada una transportando un bit. Transmitiendo en paralelo por varias líneas al mismo tiempo se puede enviar un byte (8 bits) u otros tamaños de datos de golpe.

El bus que conecta los componentes principales del computador se llama **bus del sistema (system bus)**. Este se divide en tres grupos funcionales de líneas:

![[ARQ-DE-BUSES.png]]

**1. Líneas de datos (Bus de datos)** Transportan los datos entre módulos. La cantidad de líneas se llama **anchura del bus de datos** y es un factor clave de rendimiento. Si el bus tiene 8 líneas pero las instrucciones son de 16 bits, el procesador necesita dos accesos a memoria por instrucción. Si tiene 32 o 64 líneas, puede leer instrucciones completas en un solo acceso.

**2. Líneas de dirección (Bus de dirección)** Indican la fuente o destino del dato en el bus de datos. Si el procesador quiere leer la posición 940 de memoria, pone "940" en las líneas de dirección. La **anchura del bus de dirección** determina cuánta memoria máxima puede tener el sistema (con 32 líneas de dirección se pueden direccionar 2³² = ~4 GB de memoria).

**3. Líneas de control (Bus de control)** Controlan el acceso y el uso de los otros dos buses. Transmiten señales de temporización y órdenes. Algunas señales típicas:

| Señal             | Función                                               |
| ----------------- | ----------------------------------------------------- |
| Memory Write      | Escribir dato en la posición indicada                 |
| Memory Read       | Leer dato de la posición indicada                     |
| I/O Write         | Enviar dato al puerto de E/S indicado                 |
| I/O Read          | Leer dato del puerto de E/S indicado                  |
| Transfer ACK      | Confirmar que el dato fue aceptado o puesto en el bus |
| Bus Request       | Un módulo pide el control del bus                     |
| Bus Grant         | Se cede el control del bus al módulo que lo pidió     |
| Interrupt Request | Hay una interrupción pendiente                        |
| Interrupt ACK     | La interrupción fue aceptada                          |
| Clock             | Sincroniza las operaciones                            |
| Reset             | Reinicia todos los módulos conectados                 |

Para enviar datos, un módulo debe: (1) **obtener el control del bus** y (2) **transferir el dato**. Para pedir datos, debe: (1) obtener el control, (2) enviar la petición mediante las líneas de control y dirección, y (3) esperar la respuesta.

Físicamente, el bus es un conjunto de conductores eléctricos paralelos grabados en una placa de circuito impreso. Los módulos (CPU, chips de memoria, tarjetas de E/S) se conectan al bus a través de ranuras (slots).

---

## RESUMEN ESQUEMA
~~~txt
COMPUTADOR
│
├── 4 Funciones: Procesamiento, Almacenamiento, Transferencia, Control
│
├── 4 Componentes estructurales:
│   ├── CPU
│   │   ├── Unidad de Control
│   │   ├── ALU
│   │   ├── Registros (PC, IR, AC...)
│   │   └── Interconexión interna
│   ├── Memoria Principal
│   ├── E/S
│   └── Sistema de Interconexión (BUS)
│
├── Ciclo de Instrucción:
│   ├── Captación (fetch) → IR ← Memoria[PC]; PC++
│   ├── Ejecución → Acción según IR
│   └── (con interrupciones) → Ciclo de Interrupción
│
├── Interrupciones:
│   ├── Tipos: Programa, Temporización, E/S, Hardware
│   ├── Gestión: guardar contexto → saltar a gestor → restaurar
│   └── Múltiples: desactivar ó prioridades
│
└── Bus del Sistema:
    ├── Bus de Datos → transfiere datos (anchura = rendimiento)
    ├── Bus de Dirección → indica destino/fuente (anchura = máx. memoria)
    └── Bus de Control → coordina y sincroniza todo
~~~
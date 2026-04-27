## Por qué existe la Unidad de E/S
La CPU opera con su propio clock interno, en el orden de los Ghz. Un teclado genera datos ~100 bps. Un disco rígido a ~100 millones de bps. La CPU no puede hablar directamente con dispositivos tan distintos porque: 
- **No comparten temporización**: La CPU tiene su reloj; cada dispositivo tiene el suyo o ninguno. Si la CPU espera datos del teclado al ritmo de su clock, quema millones de ciclos esperando nada. 
- **No comparten protocolo eléctrico**: Un dispositivo USB, uno serial, uno paralelo. Cada uno tiene niveles de tensión, cantidad de líneas y señales distintas. La CPU no puede tener circuitería nativa para cada uno. 
- **No comparten formato de datos**: Algunos dispositivos mandan un bit a la vez (serial), otros otros varios en paralelo. La CPU trabaja con palabras de 32 o 64 bits. 
La **Unidad de E/S** resuelve esto: actúa como traductor entre el mundo de la CPU y el mundo de los periféricos. 

## Qué hace una interfaz de E/S
Una interfaz es el circuito concreto conecta un dispositivo específico al bus del sistema. Tiene cinco responsabilidades: 
1. **Comunicación con la CPU (o memoria) a través del bus del sistema.** La interfaz aparece en el bus igual que un módulo de memoria. Tiene una dirección (o rango de direcciones. La CPU le escribe comandos y le lee los datos como si fueran posiciones  de memoria o puertos numerados.
2. **Comunicación con el dispositivo periférico**: La interfaz habla el idioma del dispositivo. Maneja sus señales eléctricas específicas y su temporización, sus protocolos mecánicos. 
3. **Control y temporización**: Coordina cuándo ocurre cada cosa. Por ejemplo, se sabe que después de enviar un comando de lectura al disco hay que esperar X tiempo antes de que el dato esté disponible. 
4. **Buffer (almacenamiento temporal)**: La interfaz tiene registros internos donde acumula datos. Si el dispositivo manda datos más rapido de lo que la CPU los consume (o viceversa), el buffer absorbe la diferencia temporalmente. Sin buffer, se perdería la información. 
5. **Detección de errores**: Verifica que los datos llegaron íntegros usando paridad, CRC u otros mecanismos dependiendo del dispositivo. 

## Clasificación de dispositivos
El módulo de E/S clasifica los dispositivos según estos criterios: 
##### **Por dirección**: 
- **Entrada**: Solo generan datos hacia la CPU (teclado, mouse, micrófono)
- **Salida**: Solo reciben datos de la CPU (monitor, impresora, parlante)
- **Bidireccional**: Ambas (disco rígido, red, memoria USB)
##### **Por velocidad/tipo de transferencia**: 
- **Orientados a caracteres**: Transfieren datos de un carácter (byte) por vez. Son lentos (teclado, puerto serial). 
- **Orientados a bloques**: Transfieren bloques de datos de tamaño fijo. Son rápidos (disco rígido, SSD, interfaz de red)
##### **Interfaces dedicadas vs genéricas**
**Dedicada**: Diseñada para un tipo específico de dispositivo (controladora de disco SATA, por ejemplo)
**Genérica**: Puede conectar distintos tipos de dispositivos (USB, PCIe)


## Transferencia interfaz - CPU
Hay dos modos de sincronización entre la interfaz y la CPU: 
##### Sincrónica
**CPU e interfaz comparten el mismo reloj**. Cada operación dura un número fijo de ciclos de reloj. Es simple pero rígido. Si el dispositivo es más lento que lo previsto, hay problemas.
##### Asincrónica
**CPU e interfaz tienen relojes independientes**. Para coordinarse, utilizan señales de control explícitas: 
- **Pulso de habilitación (strobe)**: Una parte avisa a la otra que puso datos válidos en el bus. La otra los lee cuando ve ese pulso. 
- **Handshaking**: Protocolo de reconocimiento mutuo. La CPU dice "quiero transferir", la interfaz responde "listo", la CPU manda el dato, la interfaz confirma recepción (**ACK - Acknowledge**). Si no llega confirmación en cierto tiempo, hay timeout y se reinicia o se reporta error. 

La mayoría de interfaces reales son asincrónicas porque los dispositivos periféricos tienen tiempos variables e impredecibles. 


## Transferencia Interfaz - Dispositivo
##### Serial 
Los Bits se mandan **uno por uno** sobre una (o pocas) líneas físicas. Ventajas: requiere muy poco cableado, puede cubrir largas distancias. Desventajas: más lento que paralelo para el mismo clock. Modos: 
- **Simplex**: Solo una dirección (A -> B, nunca B -> A)
- **Half-Dúplex**: Ambas direcciones, pero no simultaneamente (como un walkie-talkie)
- **Full-Dúplex**: Ambas direcciones simultáneamente (dos líneas separadas, una para cada dirección)
##### Paralelo
Se mandan múltiples bits simultáneamente sobre múltiples líneas. Por ejemplo, 8 líneas = 8 bits por ciclo de reloj. Ventajas: más rapido para el mismo clock. Desventajas: requiere mucho cableado, distancias muy cortas porque las señales se dessincronizan con la distancia (fenómeno de skew)

## Administración de E/S: Polling
En Polling (sondeo), la **CPU tiene control total y directo sobre la E/S**. El mecanismo es: 
1. La CPU ejecuta una instrucción que **lee el registro de estado** de la interfaz de E/S. 
2. Ese registro tiene bits que indican si el dispositivo está listo, ocupado, con error, etc. 
	1. La CPU evalúa ¿el dispositivo requiere atención? 
	2. Si NO, puede pasar a la siguiente interfaz o volver a preguntar a la misma. 
	3. Si SI, ejecuta la rutina de atención (lee o escribe datos, reinicia el estado, etc.)
	4. Repite el ciclo. 
La CPU **no hace nada mientras tanto**, está atrapada en este bucle de verificación. Es ineficiente debido a ello. En sistemas modernos es inaceptable, la CPU podría estar ejecutando otro proceso, pero en su lugar, está ocupada preguntando "¿ya terminaste?" en bucle. 
##### **Variante: Ronda con múltiples dispositivos**
Cuando hay varios dispositivos, la CPU los consulta en secuencia: P1, P2, P3... Pn. Existen dos variantes de qué hacer cuando encuentra uno que requiere atención. 
- **Variante A - Atender y continuar la ronda**: P1→P2→P3 (requiere atención) → atender P3 → P4→...→Pn → volver a P1
- **Variante B - Atender y volver al inicio**: P1→P2→P3 (requiere atención) → atender P3 → volver a P1
La variante B implica prioridad implícita: P1 y P2 siempre se consultan antes que P3 en adelante. Si P1 requiere atención muy frecuentemente, los dispositivos al final de la lista pueden quedar sin atención por períodos muy largos. A esto se le conoce como **starvation (inanición)**. 

La variante A es más equitativa pero tarda más en volver a atender un dispositivo urgente. 

## Administración de E/S: Interrupciones (E/S por Hardware)
Con Polling la CPU no puede hacer otra cosa. Las interrupciones invierten la lógica: **la CPU trabaja normalmente en lo suyo, y es el dispositivo quien avisa cuando necesita atención**. 
##### IRQ - Interrupt Request
Es una señal eléctrica que viaja por el bus de control, generada por la interfaz de E/S hacia la unidad de control de la CPU. Es **Asincrónica** y puede llegar en cualquier momento, sin coordinación con el clock de la CPU. 

Cuando la CPU recibe una **IRQ** no la atiende inmediatamente en ese momento exacto. Termina de ejecutar la instrucción que está procesando, y recién entonces la atiende. 
##### NMI - Non-Maskable Interrupt
Es también una señal asincrónica, pero con una diferencia crítica: **el programador no puede ignorarla ni postergarla**. Se usa para situaciones que requieren atención inmediata e inevitable. Fallo de energía, error de hardware grave, etc. 
##### Enmascaramiento 
Las interrupciones comunes (IRQ) pueden ser **enmascaradas**: el programador puede decirle a la CPU que las ignore temporalmente. Esto es necesario, por ejemplo, cuando la CPU está en medio de atender una interrupción y no quiere ser interrumpida nuevamente. 

El mecanismo usa un bit de estado de la CPU. 
- Usando el bit como **inhibición (mascara)** I=1 entonces no se atiende; I=0 entonces se atiende. 
- Usando el bit como **habilitación**: I=1 entonces se atiende. I=0 entonces no se atiende.
Ambas convenciones existen en distintas arquitecturas. Lo más importante es entender que **el bit controla si la IRQ es procesada o ignorada**. 

Las **NMI (Non-Maskable Interrupt)** no tienen ese bit, son siempre atendidas. 
##### Secuencia de atención de una interrupción
1. Termina la instrucción en curso (nunca interrumpe a la mitad)
2. Guarda estado funcional en la **pila (stack)**: PC, registros. 
3. Envía reconocimiento **ACK** al bus de control (según arquitectura)
4. Inhibe interrupciones posteriores (IE=0)
5. Obtiene dirección de la **rutina de atención (ISR - Interrupt Service Rutine)** 
6. Carga el **PC (Program Counter)** y ejecuta la **ISR (Interrupt Service Rutine)**
7. Al terminar, recupera estado de la pila y continúa 

La verificación de IRQ ocurre entre la búsqueda de operandos y la ejecución de la instrucción — **siempre termina la instrucción actual antes de atender**.

## Identificación del generador de IRQ
Cuando múltiples dispositivos comparten una línea de interrupción, la CPU debe saber quién generó la IRQ para ejecutar la ISR correcta. 
##### Consulta por software (Polling)
Después de recibir la IRQ, la CPU recorre todos los dispositivos preguntando quién interrumpió. Simple pero lento. 
##### Daisy Chain (cadena en margarita)
Todos los dispositivos comparten **una sola línea IRQ (Interrupt Request)** para notificar a la CPU. Cuando cualquiera de ellos necesita atención, pone esa línea en bajo. 

La CPU responde enviando una señal de ACK. Esta señal **viaja en cadena por los dispositivos**, de más cercano a más lejano: 
- Si el dispositivo más cercano a la CPU **fue el que interrumpió**, retiene el ACK (no lo pasa al siguiente) y coloca su vector en el bus de datos. 
- Si **no fue él**, pasa el ACK al siguiente dispositivo. 
- Y así hasta llegar al que interrumpió. 

La CPU lee el vector del bus y salta a a la ISR correspondiente. 

**Consecuencia directa**: el dispositivo más cercano a la CPU siempre recibe el ACK primero, entonces siempre tiene **mayor prioridad**. La prioridad es determinada por la posición física en la cadena, no por el software. 
##### PIC - Priority/Peripheral Interrupt Controller
Es un circuito dedicado que gestiona múltiples líneas de interrupción y presenta una sola IRQ a la CPU. Características: 
- **Análisis por hardware**: detecta qué dispositivo interrumpió sin que la CPU tenga que hacer polling. 
- **Prioridades programables por software**: a diferencia del Daisy Chain, el programador puede asignar prioridades a cada dispositivo independientemente de su posición física. 
- **Vectores programables**: el programador configura qué vector corresponde a cada dispositivo. 
- **Memoria de pendientes**: puede recordar qué interrupciones llegaron mientras que la CPU estaba atendiendo a otra.
- **Diseño en cascada**: se pueden conectar varios **PICs (Priority Interrupt Controller)** para manejar más fuentes de interrupción. 
- **Enmascaramiento selectivo**: permite inhibir individualmente cualquier fuente de interruptor. 

## DMA - Direct Access Memory 
**El problema con las interrupciones para transferencias masivas**. Con interrupciones, cada transferencia de un dato implica: **GUARDAR CONTEXTO → EJECUTAR ISR → LEER/ESCRIBIR UN DATO → RESTAURAR CONTEXTO → RETOMAR**. Para transferir un bloque de 1MB de disco a memoria, esto ocurre miles de veces. El overhead del cambio de contexto se vuelve significativo. 

Para dispositivos rápidos que transfieren grandes bloques (discos, interfaces de red, tarjetas de video) se necesita algo más eficiente. 
##### Qué es el DMA
El **DMA (Direct Access Memory)** permite que un bloque de datos viaje directamente entre un dispositivo periférico y la memoria principal, **sin que la CPU intervenga en cada transferencia individual**. 

Lo gestiona un circuito llamado **DMAC (DMA Controller)**.
1. **La CPU programa el DMAC**. Le indica: 
	- Dirección de inicio en memoria (donde empezar a leer/escribir)
	- Dirección/Identificación del dispositivo E/S
	- Cantidad de datos a transferir (longitud de bloque)
	- Dirección de la operación (leer del dispositivo y escribir en memoria, o viceversa)
2. **La CPU continúa con su trabajo normal**, ejecuta otras instrucciones. 
3. **El DMAC espera a que el dispositivo esté listo**. Cuando el dispositivo tiene datos para transferir (o está listo para recibirlos), el DMAC solicita el control de los buses mediante la señal **BR (Bus Request)**. 
4. **La CPU responde con BG (Bus Granted)**. En ese momento la CPU se **desconecta** de los buses del sistema, sus pines de bus pasan al **tercer grado** (alta impedancia), que eléctricamente equivaldría a un circuito abierto (sin paso de corriente). La CPU ya no conduce los buses. 
5. Cuando **termina la transferencia**, el DMAC notifica a la CPU con una **IRQ**. La CPU puede entonces procesar los datos ya transferidos. 
##### Técnica 1: Detencion de CPU (Burst Mode)
El DMAC toma el bus y transfiere todo el bloque sin soltarlo. La CPU queda completamente inactiva durante toda la transferencia. Ventaja: maximiza el troughput de la transferencia. Desventaja: si el bloque es grande, la CPU puede quedar inactiva por períodos largos. 

##### Técnica 2: Robo de ciclos (Cycle Stealing)
El DMAC no transfiere todo el bloque de una vez. En cambio, toma el bus, transfiere una o pocas palabras y lo suelta. La CPU puede usarlo en los ciclos intermedios. 

Mecanismo: el reloj del bus se **relentiza** durante el "robo" para dar más tiempo al DMAC. Esto es diferente a una interrupción. Es diferente a una interrupción per se, la CPU **no hace cambio de contexto**, simplemente trabaja más lento porque el bus está parcialmente ocupado. Ventaja: CPU y DMA comparten bus. Desventaja: la CPU trabaja más lento durante la transferencia (menor frecuencia efectiva de bus)

##### Configuraciones de DMA
1. **Configuración 1 - Un bus, controlador separado**: `CPU - DMAC - Interfaz E/S - Interfaz E/S - Memoria (todo en el mismo bus)`
	- Cada transferencia usa el bus **dos veces** primero el dato va de la interfaz al DMAC, luego del DMAC a la memoria. La CPU se suspende dos veces. 

2. **Configuración 2 - Un bus, controlador integrado**: `CPU - [DMAC + Interfaz E/S] - [DMAC + Interfaz E/S] - Memoria`  
	- El DMAC está integrado con la interfaz. El dato va directamente al bus en un solo viaje. La CPU se suspende una vez. 

3. **Configuración 3 - Bus separado para E/S**: `Bus del sistema: CPU - DMAC - Memoria     Bus de E/S: DMAC - Interfaz - Interfaz - Interfaz`
	- Las interfaces tienen su propio bus. El DMAC conecta ambos buses. El bus del sistema solo se usa una vez (DMAC -> Memoria). La CPU se suspende una vez. Es la configuración más eficiente. 

## Norma vs Protocolo 
Norma y Protocolo dos conceptos distintos:
- **Norma**: define el nivel **físico/eléctrico** de una interfaz. Específica: 
	- Niveles de tensión para los estados lógicos 0 y 1. 
	- Tipo y cantidad de pines/líneas. 
	- Características del conector físico. 
	- Tipo de cable y sus propiedades eléctricas. 
	- Impedancias, velocidades máximas. 

- **Protocolo**: define como se **comunican** dos entidades por software sobre una interfaz ya establecida físicamente. Específica: 
	- Cómo se inicia la conexión. 
	- Cómo se negocia la configuración. 
	- El formato y estructura de los mensajes (sintaxis).
	- El significado de cada campo y mensaje (semántica).
	- Cómo se sincronizan las dos partes. 
	- Cómo se detectan y recuperan errores.
	- Cómo se termina la conexión. 

**Relación**: La norma define el canal físico. El protocolo define cómo se usa ese canal. 

## UART - Transmisión Asincrónica
**UART (Universal Asynchronous Receiver/Transmitter)** es un circuito que implementa comunicación serial asincrónica. Su función principal es doble: 
1. **Transmisión**: toma una palabra paralela (de la CPU/Memoria) y la convierte en una secuencia de bits seriales para enviar. 
2. **Recepción**: toma una secuencia de bits seriales entrante y la reconstruye como una palabra paralela. 
##### Problema de la sincronización asincrónica
En comunicación serial sincrónica, hay un reloj compartido que le dice al receptor cuándo leer cada bit. En comunicación **asincrónica** no hay reloj compartido. Receptor y transmisor tienen relojes independientes, ¿cómo sabe el receptor cuándo empieza cada bit? Por la trama UART.
##### La trama UART
La solución es envolver cada carácter en bits especiales: 
~~~txt
[línea en ALTO]  [START=0]  [bit0] [bit1] ... [bitN]  [paridad?]  [STOP=1]  [línea en ALTO]
~~~

- **Estado de reposo**: La línea se mantiene en nivel ALTO (lógico 1). Esto indica que no se está transmitiendo nada. 

- **Bit START**: siempre es un 0 (nivel BAJO). El receptor detecta la transición de ALTO a BAJO, eso marca el comienzo de un carácter. 

- **Sincronización interna**: Cuando el UART receptor detecta el flanco de bajada del bit START activa su reloj interno y **cuenta 16 ciclos**. Esto lo lleva al centro del bit START donde lo muestrea para confirmar que efectivamente es un 0 (y no fue un pulso de ruido). Luego sigue contando 16 ciclos por cad bit para muestrear cada uno en su centro, donde la señal es más estable. 

- **Bits de datos:** los bits del carácter, del menos significativo al más significativo. Pueden ser 5, 6, 7 u 8 bits dependiendo de la configuración.

- **Bit de paridad (opcional):** un bit extra que permite detección de errores simples. Paridad par: se agrega un 1 o 0 para que la cantidad total de 1s en el carácter + paridad sea par. Paridad impar: ídem pero la cantidad total es impar. Si el receptor recibe un carácter y la paridad no coincide, sabe que hubo un error de transmisión.

- **Bit(s) STOP:** siempre en nivel ALTO (1). Pueden ser 1, 1.5 o 2 bits de STOP. Su propósito es garantizar que la línea vuelva al estado de reposo antes del próximo carácter, dando tiempo al receptor para prepararse.

##### Baudios vs. bps
**Baud** (baudios): unidad de velocidad de señalización. En UART, 1 baud = 1 bit por segundo en la línea física, contando **todos** los bits: START, datos, paridad y STOP.

**bps** (bits por segundo): cantidad de bits de **información** (datos reales) transmitidos por segundo. No cuenta START, STOP ni paridad.

## USRT y USART
**USRT (Universal Synchronous Receiver/Transmitter)** es la versión sincrónica del UART. Diferencias: 
- **No usa bits START ni STOP.** Como hay sincronización (reloj compartido o clock embebido en la señal), el receptor sabe exactamente cuándo leer cada bit. Esto elimina el overhead de los bits de control de sincronización.
- **Ocupa el canal constantemente.** En UART, entre caracteres la línea puede estar en reposo. En USRT, el canal siempre está transmitiendo algo.
- **Si no hay datos, envía el carácter SYN.** Este es un carácter especial que mantiene la sincronía sin transmitir información real — es el equivalente de "no tengo datos pero sigo aquí".
- **Mayor eficiencia** que UART para el mismo clock, porque no desperdicia bits en START/STOP.

**USART** (_Universal Synchronous/Asynchronous Receiver/Transmitter_): integra ambos circuitos (UART y USRT) en un solo chip, configurable por software para operar en modo sincrónico o asincrónico.

## Bus Mastering
Normalmente la CPU es el **Bus Master**: el único dispositivo que puede iniciar transferencias y controlar el bus. En arquitecturas simples esto es suficiente.

**Bus Mastering** es la capacidad de un dispositivo **distinto a la CPU** de tomar control del bus y comunicarse directamente con otros dispositivos sin pasar por la CPU.

DMA es una forma restringida de bus mastering: el DMAC puede tomar el bus, pero solo para transferencias que la CPU previamente programó.

**Full Bus Mastering:** el dispositivo puede realizar operaciones complejas autónomamente — iniciar transferencias, tomar decisiones, acceder a distintos recursos — sin que la CPU lo programe para cada operación. Esto requiere que el dispositivo tenga su propio microprocesador o microcontrolador. Ejemplo: una interfaz de red moderna que puede hacer DMA, calcular checksums, gestionar buffers y manejar protocolos de red — todo sin intervención de la CPU principal.

Arquitecturas como PCI soportan múltiples bus masters que se turnan el acceso al bus mediante arbitraje.

## Universal Serial Bus
USB es un bus **host-centric**: existe **un único host** (generalmente la CPU o un controlador USB dedicado en el chipset) que inicia y controla **absolutamente todas** las transacciones. Los dispositivos nunca inician comunicación por su cuenta — solo responden cuando el host los interroga.

Soporta hasta **127 dispositivos** por bus (dirección de 7 bits: 2⁷ = 128 valores, pero 0 está reservado). Se pueden conectar más usando **hubs**.

| Versión | Tasa máxima | Año  |
| ------- | ----------- | ---- |
| USB 1.0 | 1,5 Mbps    | 1996 |
| USB 1.1 | 12 Mbps     | 1998 |
| USB 2.0 | 480 Mbps    | 2000 |
| USB 3.0 | 4,8 Gbps    | 2008 |
##### Estructura de una transacción
Cada operación de transferencia se llama **transacción** y consta de hasta tres paquetes:
1. **Token Packet:** lo envía el host. Indica el tipo de transacción (IN, OUT, SETUP), la dirección del dispositivo, y el endpoint (punto de comunicación dentro del dispositivo). Es la "cabecera" que especifica qué va a ocurrir.
2. **Data Packet (opcional):** contiene los datos a transferir (el payload real). Puede estar ausente en algunas transacciones de control.
3. **Status Packet:** provee el resultado de la transacción. Incluye el ACK (transferencia exitosa), NAK (dispositivo ocupado, reintentar), o STALL (error, endpoint detenido).

## IEEE 1394 — FireWire
Bus serial de alta velocidad. A diferencia de USB, **no es host-centric** — los dispositivos pueden comunicarse entre sí directamente (peer-to-peer) sin pasar por una computadora.

**Características:**
- Topología **daisy chain**: los dispositivos se conectan en cadena, hasta 63 por puerto (1022 usando bridges)
- **Configuración automática** sin terminadores físicos
- Velocidades entre 25 y 400 Mbps (versiones posteriores llegan a varios Gbps)

**Dos modos de transferencia:**
**Asíncrono:**
- Los paquetes contienen la dirección completa del destino
- El receptor devuelve un ACK confirmando recepción
- El tamaño de los datos por paquete es variable
- Adecuado para transferencias donde importa la integridad (archivos)

**Isócrono:**
- Los paquetes tienen tamaño fijo y se envían a intervalos de tiempo regulares y garantizados
- No hay ACK — se asume que los datos llegaron
- El direccionamiento es simplificado
- Adecuado para audio y video en tiempo real, donde importa más la continuidad temporal que la retransmisión de paquetes perdidos

La combinación de ambos modos en el mismo bus es una característica distintiva de FireWire.
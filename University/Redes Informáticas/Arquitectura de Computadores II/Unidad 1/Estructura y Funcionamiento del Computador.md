> **Diferencia dato e información**: Un dato es una unidad cruda y aislada —como un número, una palabra o una observación— sin contexto ni significado completo; la información surge cuando esos datos se organizan, procesan o interpretan en un contexto que les otorga sentido y utilidad para la toma de decisiones o la comunicación.

## Sistema Jerárquico 
Un computador moderno tiene millones de componentes electrónicos. La forma de describirlo es pensarlo como un **sistema jerárquico**. Un conjunto de subsistemas interrelacionados, donde cada nivel se puede entender de forma independiente, sin necesidad de conocer los detalles del nivel inferior. 
- **Estructura**: cómo están interrelacionados los componentes entre si (la "forma" del sistema).
- **Funcionamiento**: qué hace cada componente como parte de esa estructura (el "comportamiento"). 

El enfoque que se utiliza es **top-down (de arriba hacia abajo)** se empieza por la visión más general del computador y se va bajando hacia los detalles. Es más claro y efectivo que empezar por los transistores y subir. 

## Funciones básicas del computador 
Todo computador sin importar por cuán complejo sea, se reduce a cuatro funciones: 
1. **Procesamiento de datos**: El computador transforma datos. Puede sumar, comparar, filtrar, comprimir, cifrar, etc. Aunque parece muy variado, los métodos fundamentales de procesamiento son pocos. 
2. **Almacenamiento de datos**: Hay dos tipos.
	- A **corto plazo**: mientras procesa, el computador necesita guardar temporalmente los datos con los que trabaja en ese momento (esto ocurre en la RAM o en los registros internos). 
	- A **largo plazo**: archivos que se guardan para recuperarlos más tarde (disco rígido, SSD, etc). 
3. **Transferencia de datos**: El computador necesita comunicarse con algo externo. Hay dos casos: 
	- **Entrada/Salida (E/S)** cuando el dispositivo está directamente conectado al computador (teclado, monitor, impresora). Estos se llaman **periféricos**. 
	- **Comunicación de datos**: cuando la transferencia es a larga distancia, hacia un dispositivo remoto (por red, etc)
4. **Control**: La parte que coordina todo. Dentro del computador, la **Unidad de Control** gestiona los recursos y dirige las operaciones en respuesta a las instrucciones del programa. 

## Estructura del Computador
El computador cuenta con cuatro componentes estructurales principales:
1. **CPU (Central Process Unit)**: Controla el funcionamiento de todo el computador y ejecuta el **procesamiento de datos**. También se le llama **procesador**. 
2. **Memoria Principal**: Almacena datos e instrucciones de manera temporal. Es el almacenamiento de **corto plazo** al que la CPU accede directa y rápidamente. 
3. **E/S (Entrada/Salida)**: Transfiere datos entre el computador y el entorno externo. Es el puente entre el interior (CPU + Memoria) y el mundo exterior (Usuario, red, dispositivos). 
4. **Sistema de Interconexión**: Es el mecanismo que permite la comunicación entre los tres componentes anteriores. Sin esto, la CPU no podría hablarle a la memoria ni a la E/S. 

La **CPU** es el componente más complejo e interesante. A su vez, tiene su propia estructura interna con cuatro componentes: 
1. **Unidad de Control (UC)**: Controla el funcionamiento de toda la CPU, y por lo tanto, de todo el computador. Es la que interpreta las instrucciones y les ordena a los demás componentes qué hacer. 
2. **Unidad Aritmético-Lógica (ALU)**: Realiza las operaciones matemáticas (suma, resta, multiplicación...) y lógicas (AND, OR, comparaciones...). Es el componente que realmente "procesa" los datos. 
3. **Registros**: Son memorias internas pequeñas (las más pequeñas) dentro de la CPU, extremadamente rápidas (las más rápidas). Guardan datos temporalmente con los que la CPU está trabajando en ese instante. 
	- E.g: El **Counter Program (PC)**, **Instruction Register (IR)**, **Acumulator (AC)**, etc. 
4. **Interconexiones Internas de la CPU**: Mecanismos que permiten la comunicación entre la **UC**, la **ALU** y los **registros dentro de la CPU**. 

~~~
La jerarquía completa queda así: Computador → [CPU, Memoria, E/S, Sistema de Interconexión] CPU → [Unidad de Control, ALU, Registros, Interconexión interna]
~~

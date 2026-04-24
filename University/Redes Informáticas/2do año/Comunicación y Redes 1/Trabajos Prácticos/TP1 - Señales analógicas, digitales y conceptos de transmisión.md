> 1) Graficar una señal analógica y una señal digital, indicar sus principales características y el modo por el cual transportan la información.

**Señal Analógica**: Varía de forma **continua** en el tiempo y puede tomar cualquier valor dentro de un rango. Transporta información mediante cambios en **amplitud, frecuencia o fase**. Ejemplos: voz, audio, radio AM/FM.

![[señal-analógica.png]]

**Señal Digital**: Solo puede tomar un conjunto **discreto** de valores (generalmente dos: alto = 1, bajo = 0). Transporta información mediante la **secuencia de bits**, codificada en transiciones de tensión. Ejemplos: Ethernet, USB, fibra óptica.

![[señal-digital.png]]

> 2) Indicar las cinco ventajas más notables de la transmisión digital frente a la analógica. ¿Cuál es la principal desventaja de la primera respecto de la segunda?

1. **Inmunidad al ruido.** La señal digital puede regenerarse en cada repetidor sin acumular el ruido del canal, a diferencia de la analógica que lo amplifica junto con la señal.
2. **Integración con sistemas informáticos.** La naturaleza binaria es directamente compatible con procesadores, memorias y software, facilitando el procesamiento digital de señales (DSP).
3. **Multiplexación sencilla.** Es fácil combinar múltiples flujos de datos (voz, video, datos) en un único canal mediante TDM o técnicas estadísticas.
4. **Cifrado y seguridad.** La información puede cifrarse con algoritmos estándar (AES, RSA) de forma directa, protegiendo la confidencialidad e integridad.
5. **Detección y corrección de errores.** Se pueden añadir códigos redundantes (CRC, Hamming) para detectar y corregir errores producidos durante la transmisión.

**Principal desventaja:** Mayor ancho de banda requerido. La transmisión digital de una señal analógica (p.ej., voz telefónica) exige un ancho de banda significativamente mayor que el canal analógico original, debido a la codificación (muestreo + cuantización + codificación PCM).

> 3) Qué funciones cumple un repetidor regenerativo?

Un **repetidor regenerativo** es un componente esencial en las redes digitales. Cuando una señal digital viaja por un medio físico, llega atenuada, distorsionada y con ruido; el repetidor regenerativo analiza esa señal degradada, toma una decisión lógica para identificar los bits originales y genera desde cero una señal nueva, limpia y perfectamente sincronizada. Este proceso de **muestreo, decisión y reconstrucción** evita que el ruido se acumule a lo largo del trayecto, lo que permite que los datos recorran distancias inmensas manteniendo una integridad casi perfecta.

![[repeditor-regenerativo.png]]

A diferencia de un amplificador analógico, el repetidor regenerativo no amplifica el ruido junto con la señal: descarta el ruido al reconstruir cada pulso de cero, lo que permite encadenar repetidores sin degradación acumulativa de la SNR.

![[REPETIDOR-REGENERATIVO-ESQUEMA.png]]

> 4) Dada una función periódica definir ciclo, período, frecuencia. Considerar la función $f(t) = A . sen( ω . t + φ )$

$$f(t) = A · sen(ω · t + φ)$$

![[función periódica.png]]

**Ciclo**: Unidad mínima de repetición de la señal. Es la variación completa que experimenta la función desde un estado hasta volver al mismo estado con la misma pendiente.

**Periodo ($t$)**: Tiempo que tarda en completarse un ciclo. `T = 2π / ω` (segundos). Para la señal senoidal: `T = 1/f`.

**Frecuencia ($f$)**: Número de ciclos por unidad de tiempo. `f = 1/T` (Hz). Relacionada con la frecuencia angular: `ω = 2π·f` (rad/s).

**Amplitud ($A$)**: Es el valor máximo que alcanza la señal (la "altura" de la onda). Indica cuánta energía o voltaje tiene.

**Frecuencia Angular ($ω$)**: Se mide en radianes por segundo (rad/s). Representa la velocidad a la que la señal "gira" o avanza en el círculo trigonométrico.

**Fase Inicial ($φ$)**: Indica en qué punto de su ciclo comienza la señal en el tiempo t=0. Si ϕ no es cero, la onda aparecerá desplazada hacia la izquierda o hacia la derecha.

> 5) Graficar un tren de pulsos y definir: FRP, ancho de pulso, período y amplitud del pulso.

Un **tren de pulsos** es una sucesión periódica y continua de señales rectangulares o cuadradas que se repiten a lo largo del tiempo, ampliamente utilizada en sistemas digitales y de telecomunicaciones para transportar información o actuar como señal de reloj. Se caracteriza por pasar rápidamente entre dos niveles de amplitud definidos (generalmente un nivel bajo o "0" y un nivel alto o "1"), donde cada ciclo completo está determinado por el **período (T)**, que es el tiempo total entre el inicio de un pulso y el inicio del siguiente. Dentro de este ciclo, el **ancho de pulso (τ)** define la duración efectiva del estado activo, mientras que la **FRP (Frecuencia de Repetición de Pulsos)** indica cuántos de estos pulsos ocurren en un segundo, siendo inversamente proporcional al período (FRP=1/T).

![[TREN-de-PULSOS 1.png]]

**FRP - FRECUENCIA DE REPETICIÓN DE PULSOS**: Número de pulsos por segundo. `FRP = 1 / T` (Hz o pps). Equivalente a la frecuencia de la señal periódica de pulsos.

**PERÍODO**: Tiempo entre el inicio de dos pulsos consecutivos. `T = 1 / FRP`. Incluye el ancho del pulso más el intervalo de silencio.

**ANCHO DE PULSO ($T$)**: Duración temporal de cada pulso, medida entre sus flancos de subida y bajada al 50% de la amplitud. Determina el ciclo de trabajo: `DC = τ / T`.
 
**Amplitud ($A / Vm$)**: Valor máximo de tensión (o corriente) alcanzado durante el pulso activo. Se mide desde la línea de referencia (cero) hasta el nivel alto del pulso.
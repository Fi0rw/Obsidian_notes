## ALU (Arithmetic Logic Unit)
La **ALU** es el circuito dentro del procesador que ejecuta todas las opearciones aritméticas y lógicas. No toma decisiones sobre qué hacer (eso lo decide la unidad de control). La ALU solo recibe operandos, ejecuta la operación indicada, y produce un resultado. 

##### E/S de la ALU
- **A y B**: Los dos operandos sobre los que opera. 
- **F**: Señal de control proveniente de la unidad de control que le indica qué operación realizar (suma, AND, desplazamiento, etc).
- **R**: El resultado de la operación.
- **D**: Las banderas (flags) - bits que informan sobre el resultado. 

##### Flags (Banderas)
Las flags son bits que informan sobre el resultado. Son bits individuales almacenados en un registro especial (PSW o CCR según la arquitectura). Se activan o desactivan según el resultado de la operación. 
- **Carry (C)**: Se activa cuando hay acarreo saliente del bit más significativo en una operación sin signo. También se usa en restas como "borrow". 
- **Auxiliary Carry**: Acarreo entre los bits 3 y 4. Se usa en aritmética BCD. 
- **Overflow (V u O)**: Se activa cuando el resultado es demasiado grande para caber en el registro de destino, considerando números con signo. 
- **Zero (Z)**: Se activa cuando el resultado es exactamente 0. 
- **Parity (P)**: Bit de paridad, se activa según la paridad del resultado (par o impar cantidad de bits en 1)
- **Borrow**: En algunas arquitecturas es equivalente al carry pero específico para restas. 
Estos flags son los que después la unidad de control usa para tomar decisiones. 

## Números con signo y coma
El hardware solo maneja 0s y 1s. Un número como -13,3412 tiene dos problemas: 
- Es **negativo**, no es un dígito (información adicional).
- La **coma** tampoco es un dígito, indica una posición, no un valor almacenable. 
Para representar enteros con signo existen tres sistemas. Para representar números comparte decimal existen otros dos. 

## Representación de enteros con signo
##### Signo y Magnitud
El bit más significativo (MSB, el de más a la izquierda) se reserva para el signo:

- 0 → positivo
- 1 → negativo

Los bits restantes representan el valor absoluto (magnitud) en binario normal.

Ejemplo con 8 bits:

```
+18 = 0|0010010
-18 = 1|0010010
       ↑ bit de signo
```

**Limitaciones que lo hacen inútil en la práctica:**

**1. El cero tiene dos representaciones:**

```
+0 = 00000000
-0 = 10000000
```

Esto complica cualquier verificación de "¿el resultado es cero?", porque el hardware tendría que comparar contra dos patrones distintos.

**2. La aritmética es compleja:** sumar dos números en signo-magnitud requiere primero comparar los signos, luego comparar las magnitudes, y decidir si sumar o restar las magnitudes y qué signo poner al resultado. El circuito necesario es complicado y lento.

Por estas razones **no se usa en ALUs reales** para operaciones con enteros.

##### Complemento a Uno (Ca1)
El MSB sigue siendo el bit de signo (0=positivo, 1=negativo).
- Los positivos se representan igual que en binario natural.
- Los negativos: se toma la representación positiva y se **invierte cada bit** (0→1, 1→0).

Ejemplo:
```
+25 = 0|11001  (7 bits, MSB=0, magnitud=11001 que es 25)
-25 = 1|00110  (se invirtieron los 6 bits de magnitud)
```

Sigue teniendo el problema del doble cero:
```
+0 = 00000000
-0 = 11111111
```

Tampoco se usa en la práctica.

##### Complemento a Dos (Ca2)
Es el sistema que usan **todos los procesadores modernos** para enteros con signo.

**Interpretación directa del valor:**
Para un número de n bits, el MSB tiene peso **-2^(n-1)** en lugar de +2^(n-1). Los demás bits tienen sus pesos positivos normales.

$$A=−2n−1⋅an−1+∑i=0n−22i⋅aiA = -2^{n-1} \cdot a_{n-1} + \sum_{i=0}^{n-2} 2^i \cdot a_iA=−2n−1⋅an−1​+i=0∑n−2​2i⋅ai​$$

Ejemplo con 4 bits:
```
1111 = -1×2³ + 1×2² + 1×2¹ + 1×2⁰ = -8 + 4 + 2 + 1 = -1
1000 = -1×2³ + 0×2² + 0×2¹ + 0×2⁰ = -8 + 0 + 0 + 0 = -8
0111 = -0×2³ + 1×2² + 1×2¹ + 1×2⁰ =  0 + 4 + 2 + 1 = +7
0000 = 0
```

**Rango con n bits:** de −2^(n-1) hasta +2^(n-1)−1#

Con 4 bits: de -8 hasta +7. Con 8 bits: de -128 hasta +127.

Nótese la asimetría: hay un negativo más que positivos. Esto es porque el cero tiene **una sola representación** (todo ceros), lo que es una ventaja.

**Por qué es superior:**

La suma funciona exactamente igual que en binario sin signo — se suman bit a bit con acarreo. La ALU no necesita circuitos especiales para manejar el signo. Suma de positivos, suma de negativos, suma de mixtos — todo usa el mismo sumador binario.

---

### 4. Operaciones aritméticas con enteros en Ca2

#### 4.1 Negación (cambiar el signo)

Para obtener el Ca2 de un número (es decir, su negativo):

1. **Invertir todos los bits** (complemento booleano, que es Ca1)
2. **Sumar 1** al resultado

Ejemplo: negar +18

```
+18  =  00010010
Ca1  =  11101101   (invertir todos los bits)
+1   =          1
Ca2  =  11101110   → esto es -18
```

Verificación: 11101110 = -128 + 64 + 32 + 8 + 4 + 2 = -128 + 110 = -18 ✓

**Casos especiales:**

Negación del 0:

```
00000000 → Ca1 → 11111111 → +1 → 100000000
```

El resultado tiene 9 bits pero el registro es de 8 — el bit extra (acarreo del MSB) se descarta. Queda 00000000. El opuesto del 0 es 0. Correcto.

Negación de -128 (10000000 en 8 bits):

```
10000000 → Ca1 → 01111111 → +1 → 10000000
```

Vuelve a dar -128. Esto es la anomalía mencionada en el PDF: -128 no tiene representación positiva en 8 bits (el +128 requeriría 9 bits). Esta es la única excepción que el programador debe evitar.

---

#### 4.2 Suma en Ca2

Se realiza igual que suma binaria normal. Se suman bit a bit de derecha a izquierda propagando el acarreo.

Ejemplos del PDF (4 bits):

```
(a) (-7) + (+5):
  1001  =  -7
+ 0101  =  +5
──────
  1110  =  -2   ✓
```

```
(b) (-4) + (+4):
  1100  =  -4
+ 0100  =  +4
──────
 10000  → acarreo del MSB se descarta → 0000 = 0  ✓
```

El acarreo que sale del MSB en Ca2 simplemente se ignora cuando ocurre en sumas mixtas o de distintos signos.

---

#### 4.3 Desbordamiento (Overflow) en suma

El overflow ocurre cuando el resultado matemáticamente correcto no cabe en el número de bits disponibles.

**Regla:** el overflow solo puede ocurrir cuando se suman **dos números del mismo signo**. Si el resultado tiene signo opuesto a los operandos, hay overflow.

Ejemplos del PDF (4 bits, rango -8 a +7):

```
(e) (+5) + (+4):
  0101  =  +5
+ 0100  =  +4
──────
  1001  → MSB = 1, signo negativo → DESBORDAMIENTO
  (5 + 4 = 9, que excede el máximo +7)
```

```
(f) (-7) + (-6):
  1001  =  -7
+ 1010  =  -6
──────
 10011  → descarto acarreo → 0011 = +3 → DESBORDAMIENTO
  (-7 + (-6) = -13, que excede el mínimo -8)
```

Cuando hay overflow, el flag V (Overflow) se activa.

---

#### 4.4 Resta en Ca2

La resta **A − B** se implementa como **A + (−B)**, es decir:

1. Calcular el Ca2 de B (negarlo)
2. Sumar A + (−B) normalmente

Esto permite que el mismo circuito sumador de la ALU maneje tanto sumas como restas — solo se necesita agregar un negador al camino de uno de los operandos.

Ejemplo del PDF (a): M = 2, S = 7 → M − S = 2 − 7

```
M  = 0010  =  +2
S  = 0111  =  +7
-S = 1001  =  -7   (Ca2 de S)

0010
+1001
──────
1011  = -5   ✓  (2 - 7 = -5)
```

---

#### 4.5 Multiplicación en Ca2

La multiplicación en binario funciona igual que en decimal: multiplicar el multiplicando por cada bit del multiplicador, desplazando, y sumar los productos parciales.

**Reglas para el signo en Ca2:**

- **Ambos positivos:** operar directamente. El resultado es positivo.
- **Ambos negativos:** convertir ambos a positivos (Ca2 de cada uno), multiplicar, el resultado es positivo.
- **Uno negativo:** convertir el negativo a positivo (Ca2), multiplicar por el otro, al resultado aplicarle Ca2 → ese resultado es negativo.

Ejemplo del PDF: 9 × 13

```
  1001   (9)
× 1101  (13)
────────
  1001        (9 × 1, posición 0)
  0000        (9 × 0, desplazado 1)
  1001        (9 × 1, desplazado 2)
+ 1001        (9 × 1, desplazado 3)
──────────
1110101  (117)  ✓
```

---

#### 4.6 División en Ca2

Igual que en decimal: división larga. Las mismas reglas de signo que la multiplicación aplican.

---

#### 4.7 Extensión de longitud de bits

Cuando se necesita representar un número de n bits en un registro de m bits (m > n), se deben agregar bits a la izquierda. La regla es:

- Si el número es **positivo** (MSB = 0): rellenar con **ceros** a la izquierda.
- Si el número es **negativo** (MSB = 1): rellenar con **unos** a la izquierda.

Esto se llama **extensión de signo** y preserva el valor del número.

Ejemplo: extender -5 (1011 en 4 bits) a 8 bits:

```
1011 → 11111011   (se rellenan con unos porque el MSB era 1)
```

Verificación: 11111011 = -128+64+32+16+8+2+1 = -5 ✓

---

### 5. Representación de números con parte decimal

#### 5.1 Punto fijo

Se decide de antemano cuántos bits representan la parte entera y cuántos la parte decimal, y esa división **nunca cambia**.

Notación: **Qm,n** donde m = bits para la parte entera, n = bits para la parte decimal. Se necesita además 1 bit de signo. Total: m + n + 1 bits.

Ejemplo: en un sistema Q20,12 con 32 bits totales (más 1 signo), los 20 bits de mayor peso representan la parte entera y los 12 de menor peso representan la parte fraccionaria (cada bit tiene peso 2^-1, 2^-2, ..., 2^-12).

**Ventaja:** la aritmética es simple — las operaciones son iguales a las de enteros, solo hay que recordar dónde está la coma implícita.

**Desventajas:**

- El rango está fijo. No puede representar números muy grandes ni fracciones muy pequeñas simultáneamente.
- En divisiones entre números grandes, la parte fraccionaria del cociente puede no tener bits suficientes para representarse y se pierde.

---

#### 5.2 Punto flotante

En lugar de fijar la posición de la coma, se representa el número como:

N=±M×B±EN = \pm M \times B^{\pm E}N=±M×B±E

Donde:

- **M** = mantisa (parte significativa): los dígitos del número
- **B** = base (generalmente 2 en hardware)
- **E** = exponente: determina dónde está la coma

El exponente puede ajustarse libremente, de ahí el nombre "flotante" — la coma "flota" a donde sea necesario.

Ejemplos del PDF:

```
2547,35  =  2,54735 × 10³     (base 10)
0,0035   =  3,5    × 10⁻³

111,0110₂ =  1,110110 × 2²    (base 2)
0,001101₂ =  1,101   × 2⁻³
```

El mismo número puede expresarse de múltiples formas:

```
0,110 × 2⁵  =  110 × 2²  =  0,0110 × 2⁶   (todos equivalentes)
```

Para tener una única representación canónica, se usa la **normalización**.

---

#### 5.3 Normalización

Un número en punto flotante en base 2 está normalizado cuando su parte significativa tiene la forma:

±0,1bbb…b×2±E\pm 0,1bbb\ldots b \times 2^{\pm E}±0,1bbb…b×2±E

Es decir, el primer bit después de la coma es siempre 1. Esto garantiza representación única y maximiza los bits de precisión (no se desperdicia ningún bit en ceros iniciales).

En IEEE 754 se usa una convención equivalente pero expresada como 1.M — el bit 1 antes de la coma está **implícito** y no se almacena (se llama _hidden bit_ o bit oculto). Esto da 1 bit extra de precisión gratis.

---

### 6. IEEE 754

Es el estándar adoptado en 1985 que usan prácticamente todos los procesadores actuales. Define el formato exacto, las operaciones, el redondeo y los casos especiales.

#### 6.1 Precisión Simple (32 bits)

```
[1 bit signo | 8 bits exponente | 23 bits mantisa]
```

**Fórmula para números normalizados:**

N=(−1)s×2E−127×1.MN = (-1)^s \times 2^{E-127} \times 1.MN=(−1)s×2E−127×1.M

- **s:** bit de signo (0=positivo, 1=negativo)
- **E:** el exponente almacenado (valor sesgado), entre 1 y 254
- **M:** los 23 bits de la parte fraccionaria (el "1." inicial está implícito)
- **Sesgo:** 127. El exponente real es E − 127, así que puede representar exponentes reales de −126 a +127.

**Por qué el sesgo (biased exponent):** En lugar de guardar el exponente en Ca2, se le suma 127 y se guarda como entero sin signo. Esto permite comparar dos números flotantes comparando sus bits como si fueran enteros sin signo — útil para ordenar y comparar sin lógica especial.

**Casos especiales:**

- **E = 0, M = 0:** el número es **cero** (excepción, no sigue la fórmula normal)
- **E = 255:** representa **infinito** (overflow) o NaN (_Not a Number_)
- **E = 0, M ≠ 0:** número **no normalizado** (desnormalizado), la fórmula es (−1)s×2−126×0.M(-1)^s \times 2^{-126} \times 0.M (−1)s×2−126×0.M — permite representar números muy pequeños cercanos al cero

---

#### 6.2 Precisión Doble (64 bits)

```
[1 bit signo | 11 bits exponente | 52 bits mantisa]
```

N=(−1)s×2E−1023×1.MN = (-1)^s \times 2^{E-1023} \times 1.MN=(−1)s×2E−1023×1.M

- Sesgo: 1023
- E entre 1 y 2046
- E = 2047: infinito o NaN
- E = 0: cero o desnormalizados

---

#### 6.3 Comparación

|Parámetro|Simple (32 bits)|Doble (64 bits)|
|---|---|---|
|Bits de exponente|8|11|
|Sesgo|127|1023|
|Bits de mantisa|23|52|
|Exponente máximo real|+127|+1023|
|Exponente mínimo real|-126|-1022|
|Rango aprox. (base 10)|10⁻³⁸ a 10⁺³⁸|10⁻³⁰⁸ a 10⁺³⁰⁸|

---

#### 6.4 Ejemplo completo: convertir -7,625 a IEEE 754 simple

**Paso 1:** Convertir la parte entera a binario.

```
7 ÷ 2 = 3 resto 1
3 ÷ 2 = 1 resto 1
1 ÷ 2 = 0 resto 1
→ 7₁₀ = 111₂
```

**Paso 2:** Convertir la parte decimal a binario.

```
0,625 × 2 = 1,250 → bit 1
0,250 × 2 = 0,500 → bit 0
0,500 × 2 = 1,000 → bit 1
→ 0,625₁₀ = 0,101₂
```

Por tanto: -7,625₁₀ = -111,101₂

**Paso 3:** Normalizar. Desplazar la coma hasta que quede después del primer 1:

```
111,101 = 1,11101 × 2²
```

El exponente real es 2.

**Paso 4:** Calcular los tres campos:

```
s = 1                          (negativo)
E = 2 + 127 = 129 = 10000001₂  (exponente sesgado)
M = 11101                      (parte fraccionaria, rellenar con ceros hasta 23 bits)
M = 11101000000000000000000
```

**Resultado final (32 bits):**

```
1 | 10000001 | 11101000000000000000000
↑     ↑              ↑
s     E               M
```

---

### 7. Aritmética en punto flotante

#### 7.1 Suma y Resta

Son las operaciones **más complejas** en punto flotante porque los exponentes deben igualarse antes de operar sobre las mantisas.

**Pasos para la suma:**

1. **Igualar exponentes:** el número con el exponente menor se desplaza (denormaliza) hacia la derecha tantas posiciones como diferencia haya entre exponentes. Esto puede causar pérdida de bits por la derecha (_agotamiento de la parte significativa_).
2. **Sumar las partes significativas.**
3. **Normalizar el resultado:** si la suma produce un acarreo en el bit más significativo, desplazar a la derecha y ajustar el exponente.

**Pasos para la resta:** igual, pero en el paso 2 se restan. La complejidad adicional es que puede requerir convertir a Ca2.

---

#### 7.2 Multiplicación y División

Son operaciones **más simples** que la suma:

**Multiplicación:**

1. Sumar los exponentes (reales, es decir restar el sesgo una vez): Er=E1+E2−sesgoE_r = E_1 + E_2 - sesgo Er​=E1​+E2​−sesgo
2. Multiplicar las mantisas.
3. Redondear y normalizar.

**División:**

1. Restar los exponentes: Er=E1−E2+sesgoE_r = E_1 - E_2 + sesgo Er​=E1​−E2​+sesgo
2. Dividir las mantisas.
3. Redondear y normalizar.

---

#### 7.3 Situaciones de error en punto flotante

|Situación|Causa|Consecuencia|
|---|---|---|
|**Desbordamiento del exponente**|El exponente positivo excede el máximo representable|Error grave; se representa como infinito (E=255 en simple)|
|**Agotamiento del exponente**|El exponente negativo es menor que el mínimo representable|El número es demasiado pequeño; se trata como cero|
|**Agotamiento de la parte significativa**|Al igualar exponentes en suma/resta, se desplazan bits fuera del campo de mantisa|Pérdida de precisión en los bits menos significativos|

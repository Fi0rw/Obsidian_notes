## 1. LÍMITES — Conceptos base

- [ ] **Límite lateral izquierdo** → `lím x→a⁻ f(x)`
- [ ] **Límite lateral derecho** → `lím x→a⁺ f(x)`
- [ ] El límite **existe** solo si `lím⁻ = lím⁺`
- [ ] El límite existe aunque `f(a)` no esté definida

---

## 2. INDETERMINACIONES y cómo resolverlas

|Indeterminación|Técnica|
|---|---|
|`0/0`|Factorizar, racionalizar, L'Hôpital|
|`∞/∞`|Dividir por mayor potencia, L'Hôpital|
|`∞ - ∞`|Factor común, racionalizar|
|`0 · ∞`|Reescribir como `0/0` o `∞/∞`|
|`1^∞`|Forma de Euler: `e^(lím)`|
|`0^0`|Logaritmo + exponencial|
|`∞^0`|Logaritmo + exponencial|

---

## 3. LÍMITES TRIGONOMÉTRICOS

- [ ] `lím x→0 sen(x)/x = 1` ← **fundamental**
- [ ] `lím x→0 (1 - cos(x))/x = 0`
- [ ] `lím x→0 tan(x)/x = 1`
- [ ] `lím x→0 sen(ax)/bx = a/b` ← generalización
- [ ] **Truco:** si no es `x→0`, hacer sustitución `u = x - a`

---

## 4. LÍMITE DE EULER

$$e = \lim_{x \to \infty} \left(1 + \frac{1}{x}\right)^x$$

- [ ] Forma general: `lím (1 + f(x))^(g(x))` con `f→0` y `g→∞`
- [ ] Reescribir siempre que la base → 1 y el exponente → ∞
- [ ] Resultado: `e^(lím x→∞ f(x)·g(x))`

> **Ejemplo tipo:** `lím (1 + 2/x)^x = e²`

---

## 5. LÍMITES FINITOS e INFINITOS

|Tipo|Descripción|
|---|---|
|**Finito en punto finito**|`lím x→a f(x) = L` (L real)|
|**Infinito en punto finito**|`lím x→a f(x) = ±∞` → Asíntota vertical|
|**Finito en el infinito**|`lím x→∞ f(x) = L` → Asíntota horizontal|
|**Infinito en el infinito**|`lím x→∞ f(x) = ±∞` → Ver asíntota oblicua|

---

## 6. FUNCIONES A TRAMOS — Límites

- [ ] Calcular límite lateral en cada **punto de quiebre**
- [ ] Verificar que `lím⁻ = lím⁺` para que exista el límite
- [ ] No confundir **existencia del límite** con **continuidad**

```
f(x) = { x²      si x < 1
        { 2x - 1  si x ≥ 1
→ Calcular lím x→1⁻ y lím x→1⁺ por separado
```

---

## 7. CONTINUIDAD

Una función es **continua en x = a** si se cumplen las **3 condiciones**:

- [ ] **1.** `f(a)` está definida
- [ ] **2.** `lím x→a f(x)` existe
- [ ] **3.** `lím x→a f(x) = f(a)`

---

## 8. DISCONTINUIDADES

|Tipo|Característica|
|---|---|
|**Evitable**|El límite existe pero `≠ f(a)` (o `f(a)` no existe)|
|**Salto (1er especie)**|Los límites laterales existen pero `lím⁻ ≠ lím⁺`|
|**Esencial (2da especie)**|Al menos un límite lateral es `±∞` o no existe|

---

## 9. ASÍNTOTAS — Clasificación por límites

### 🔴 Vertical (AV)

```
lím x→a⁺ f(x) = ±∞   o   lím x→a⁻ f(x) = ±∞
```

- [ ] Buscar donde el denominador se anula
- [ ] Verificar que el numerador **no** también se anule (si no, puede ser evitable)

### 🔵 Horizontal (AH)

```
lím x→+∞ f(x) = L   o   lím x→-∞ f(x) = L
```

- [ ] Si el límite da un número real → `y = L` es AH

### 🟡 Oblicua (AO)

```
m = lím x→∞ f(x)/x        (debe ser finito y ≠ 0)
b = lím x→∞ [f(x) - mx]   (debe ser finito)
→ Asíntota: y = mx + b
```

- [ ] Solo existe si **no hay asíntota horizontal**
- [ ] Si `m = 0` → sería horizontal, no oblicua

---

## 10. DERIVADA — Definición

### Con punto:

$$f'(x) = \lim_{x \to 0} \frac{f(x) - f(c)}{x - c}$$

### Sin punto (función general):

$$f'(x) = \lim_{x \to 0} \frac{(\delta_{x} + x) - f(x)}{\delta_{x}}$$

---

## 11. RECTA TANGENTE

$$m_{t} = f'(x)$$

$$y = m_{t}(x - c) + f(x)$$

- [ ] **Paso 1:** Calcular `f(a)` → punto de tangencia
- [ ] **Paso 2:** Calcular `f'(a)` → pendiente
- [ ] **Paso 3:** Reemplazar en la fórmula

> ⚠️ Si piden recta **normal**: pendiente = `-1/f'(a)`

---

## 12. TABLA DE DERIVADAS

|Función|Derivada|
|---|---|
|`k` (constante)|`0`|
|`xⁿ`|`n · xⁿ⁻¹`|
|`eˣ`|`eˣ`|
|`aˣ`|`aˣ · ln(a)`|
|`ln(x)`|`1/x`|
|`logₐ(x)`|`1 / (x · ln(a))`|
|`sen(x)`|`cos(x)`|
|`cos(x)`|`-sen(x)`|
|`tan(x)`|`1/cos²(x) = sec²(x)`|
|`arcsen(x)`|`1 / √(1 - x²)`|
|`arccos(x)`|`-1 / √(1 - x²)`|
|`arctan(x)`|`1 / (1 + x²)`|
|`√x`|`1 / (2√x)`|
|`1/x`|`-1/x²`|

---

## 13. REGLA DE LA CADENA

$$[f(g(x))]' = f'(g(x)) \cdot g'(x)$$

- [ ] Identificar función **exterior** e **interior**
- [ ] Derivar exterior (dejando interior igual) × derivar interior

```
[sen(x²)]'  = cos(x²) · 2x
[e^(3x)]'   = e^(3x)  · 3
[ln(x²+1)]' = 1/(x²+1) · 2x
[(x²+1)⁵]' = 5(x²+1)⁴ · 2x
```

---

## RECORDATORIOS 

- [ ] En límites con raíces → **racionalizar** multiplicando por el conjugado
- [ ] `0/0` → intentar **factorizar** primero antes de L'Hôpital
- [ ] Para continuidad en tramos → evaluar siempre en los **puntos de quiebre**
- [ ] Asíntota vertical → verificar **ambos lados** del límite
- [ ] Derivada por definición puede pedirse con o sin punto → leer bien el enunciado
- [ ] Recta tangente siempre necesita **punto** y **pendiente**

---

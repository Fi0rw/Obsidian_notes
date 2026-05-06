## Medición
Medir es **comparar una magnitud física con un patrón de referencia establecido o conocido**. En toda medición intervienen tres elementos: 
1. El objeto a medir
2. El instrumento de medida (que posee el error propio de sus componentes)
3. La unidad de medida 
En la práctica, **ningún instrumento de medición es perfecto**, siempre hay errores propios de los elementos de medida. Todo voltímetro, amperímetro o multímetro tiene tolerancias, desgaste, temperatura ambiente, y otros factores que introducen **errores** en cada medición. Por eso, en ciencia e ingeniería nunca se confía de una sola medición. Se toman varias, se promedia, y se calcula el error. 

---
## Errores de medición
Como es imposible conocer el valor de una magnitud con exactitud absoluta, **se acepta como valor de referencia el promedio de varias mediciones**, esto ultimo es conocido como **valor real**. Donde:
- $x_{1},x_{2},…,x_{n}$ → son cada una de las mediciones realizadas
- $n$ → es la cantidad total de mediciones
- $x_{p}$ → es el **valor promedio** tomado como **valor real**

$$x_{p} =  \frac{x_{1} + x_{2} + x_{3} + \dots + x_{n}}{n}$$

##### Error Absoluto
Es la **diferencia entre cada medición individual y el valor promedio**. El error absoluto puede ser positivo o negativo dependiendo de si la medición estuvo por encima o por debajo del promedio. Donde: 
- $x_{i}$ → es la medición individual
- $x_{p}$ → es el promedio/valor real 

$$E_{a} = | x_{i} - x_{p} |$$

##### Error Relativo 
El error relativo es **la división entre el error absoluto ($E_{a}$) y el valor promedio $(x_{p})$**. El error relativo no tiene unidades y sirve para comparar la "gravedad" de la impresición del error que hubo en la medición. 

$$E_{r} = \frac{E_{a}}{x_{p}} $$


##### Error Porcentual
El error porcentual es **el error relativo multiplicado por cien**. Su función principal es básicamente mejorar la comunicación y la interpretación rápida, debido a que es expresada en porcentaje. 

$$E_{p} = \frac{E_{a}}{x_{p}} \times 100$$
---

## Sistemas de Medición 
Internacionalmente existen varios sistemas de medición. Los tres principales son **MKS**, **CGS** y el **Sistema Técnico**. 

1. **Sistema MKS**: 
	- Nombre: **M**etro, **K**ilogramo, **S**egundo
	- Las unidades base que le dan el nombre son exactamente esas
	- Es el predecesor directo del **SI** (Sistema Internacional de Unidades), que es el estándar mundial actual. 
	- Cuando se dice "Kilogramo" en este sistema, se habla de **kilogramo-masa** (cantidad de materia), NO de peso. 

2. **Sistema CGS**
	- Nombre: **C**entímetro, **G**ramo, **S**egundo
	- Usado históricamente en la física, química y algunas ramas de la electrónica. 
	- Más cómodo para escalas pequeñas (microscópicas)

3. **Sistema Técnico**
	- Mantiene el **metro** para longitud y el **segundo** para el tiempo. 
	- Pero la masa se mide en **UTM** (Unidad Técnica de Masa), una unidad derivada de la fuerza. 
	- La fuerza se mide en **kgf** (Kilogramo-fuerza), que es lo que comúnmente se le llama "peso" en la vida cotidiana. 

| Magnitud    | MKS        | Sistema Técnico | CGS        |
| ----------- | ---------- | --------------- | ---------- |
| Longitud    | Metro      | Metro           | Centímetro |
| Masa        | Kg masa    | UTM             | Gramo      |
| Tiempo      | Segundo    | Segundo         | Segundo    |
| Fuerza      | Newton (N) | Kgf             | Dyna (Dy)  |
| Aceleración | m/seg²     | M/seg²          | Cm/seg²    |

**Masa vs Peso**: La Masa es la cantidad de materia que tiene un cuerpo, el Peso es la fuerza con la que la tierra atrae un cuerpo. Se relacionan $F_{peso} = m_{masa} \times g_{\text{gravedad m/s²}}$

Consecuencias prácticas: 
- Un cuerpo con masa de 10 kg tiene siempre 10kg de masa, en la Tierra, en la Luna, en el espacio, etc. 
- Pero su **peso** en la Luna sería mucho menor porque $g_{Luna} ≈ 1.62\text{m/s²}$ comparada a la gravedad $9.8\text{m/s²}$ en la tierra
- Cuando en el supermercado dicen "un kilo de pan" técnicamente se está hablando del peso ($kgf$), no de masa, aunque en la vida cotidiana se use como sinónimo. 
- Otro dato sería que $g$ no es constante en toda la Tierra, varía según la distancia al Ecuador (en el Ecuador es levemente menor que en los polos). Por ello, 1kg de pan en Colombia no pesa exactamente lo mismo que en Argentina (aunque la diferencia sea mínima) 

---

## Unidades Eléctricas
| Símbolo | Nombre           | Magnitud que mide                           |
| ------- | ---------------- | ------------------------------------------- |
| **V**   | Volt (Voltio)    | Tensión eléctrica (diferencia de potencial) |
| **A**   | Ampere (Amperio) | Corriente eléctrica                         |
| **W**   | Watt (Vatio)     | Potencia eléctrica activa                   |
| **VA**  | Volt-Ampere      | Potencia eléctrica aparente                 |
| **Ω**   | Ohm              | Resistencia eléctrica                       |
| **Hz**  | Hertz            | Frecuencia                                  |

---

## Múltiplos y Submúltiplos
Como las magnitudes eléctricas pueden variar en rangos enormes (desde millonésimas hasta millones), se usan prefijos para evitar escribir muchos ceros. 

- **Múltiplos**: valores mayores a 1

| Prefijo    | Símbolo | Multiplicador     | Notación Científica |
| ---------- | ------- | ----------------- | ------------------- |
| Tera       | T       | 1 000 000 000 000 | 1 × 10¹²            |
| Giga       | G       | 1 000 000 000     | 1 × 10⁹             |
| Mega       | M       | 1 000 000         | 1 × 10⁶             |
| Kilo       | k       | 1 000             | 1 × 10³             |
| **Unidad** | —       | **1**             | **1 × 10⁰**         |

- **Submúltiplos**: valores menores a 1

| Prefijo | Símbolo | Multiplicador     | Notación Científica |
| ------- | ------- | ----------------- | ------------------- |
| mili    | m       | 0.001             | 1 × 10⁻³            |
| micro   | μ (u)   | 0.000 001         | 1 × 10⁻⁶            |
| nano    | n       | 0.000 000 001     | 1 × 10⁻⁹            |
| pico    | p       | 0.000 000 000 001 | 1 × 10⁻¹²           |


---

## Notación Científica Aplicada
La notación científica se expresa en cualquier número como: 

$$N = a \times 10^n$$
Donde $1 <= |a| < 10$ y $n$ es un entero (positivo o negativo).

| Forma extendida | Con prefijo         | Notación científica |
| --------------- | ------------------- | ------------------- |
| 1 000 000 V     | 1 MV (MegaVolt)     | 1 × 10⁶ V           |
| 1 000 Ω         | 1 kΩ (KiloOhm)      | 1 × 10³ Ω           |
| 0.001 A         | 1 mA (MiliAmpere)   | 1 × 10⁻³ A          |
| 0.000 001 A     | 47 μA (MicroAmpere) | 47 × 10⁻⁶ A         |
| 0.000 000 001 A | 1 nA (NanoAmpere)   | 1 × 10⁻⁹ A          |
| 2 200 Ω         | 2.2 kΩ              | 2.2 × 10³ Ω         |
| 47 000 Ω        | 47 kΩ               | 4.7 × 10⁴ Ω         |
| 0.0047 A        | 4.7 mA              | 4.7 × 10⁻³ A        |
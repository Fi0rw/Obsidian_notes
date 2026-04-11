
$$1) \lim_{x\to-2} \frac{2x^4 - 16x^2 + 32}{x^3 - 2x^2 - 5x + 6}$$

- **Evaluación inicial:** Al sustituir $x = -2$, se obtiene la indeterminación $\frac{0}{0}$.
- **Factorización del numerador:** Se extrae factor común y se identifica un trinomio cuadrado perfecto:
    - $2(x^4 - 8x^2 + 16) = 2(x^2 - 4)^2 = 2[(x-2)(x+2)]^2 = 2(x-2)^2(x+2)^2$.
- **Factorización del denominador:** Aplicando la regla de Ruffini para la raíz $x = -2$:
	- $(x+2)(x^2-4x+3) = (x+2)(x-3)(x-1)$ .
- **Simplificación:**
  $$\lim_{x\to-2} \frac{2(x-2)^2(x+2)^{\cancel{2}}}{\cancel{(x+2)}(x-3)(x-1)} = \lim_{x\to-2} \frac{2(x-2)^2(x+2)}{(x-3)(x-1)}$$
$$\frac{2(-2-2)^2(-2+2)}{(-2-3)(-2-1)} = \frac{2(-4)^2(0)}{(-5)(-3)} = \frac{0}{15}$$
$$\lim_{x\to-2} \frac{2x^4 - 16x^2 + 32}{x^3 - 2x^2 - 5x + 6} = 0$$

---

$$2)  \lim_{x\to 1} \frac{1-\sqrt{2-x}}{4-2\sqrt{3x+1}}$$
- **Evaluación inicial:** Sustituyendo $x = 1$ se obtiene $\frac{0}{0}$.
- **Racionalización:** Multiplicamos por los conjugados del numerador y del denominador para eliminar las raíces:
  $$\lim_{x\to 1} \frac{(1-\sqrt{2-x})}{2(2-\sqrt{3x+1})} \cdot \frac{(1+\sqrt{2-x})}{(1+\sqrt{2-x})} \cdot \frac{(2+\sqrt{3x+1})}{(2+\sqrt{3x+1})}$$$$\lim_{x\to 1} \frac{1-(2-x)}{2(4-(3x+1))} \cdot \frac{2+\sqrt{3x+1}}{1+\sqrt{2-x}} = \lim_{x\to 1} \frac{x-1}{2(3-3x)} \cdot \frac{2+\sqrt{3x+1}}{1+\sqrt{2-x}}$$$$\lim_{x\to 1} \frac{\cancel{x-1}}{-6\cancel{(x-1)}} \cdot \frac{2+\sqrt{3x+1}}{1+\sqrt{2-x}}$$$$\frac{1}{-6} \cdot \frac{2+\sqrt{3(1)+1}}{1+\sqrt{2-1}} = \frac{1}{-6} \cdot \frac{4}{2} = \frac{4}{-12}$$$$\lim_{x\to 1} \frac{1-\sqrt{2-x}}{4-2\sqrt{3x+1}} = -\frac{1}{3}$$
---


$$3) \lim_{x\to\infty} e^x$$

- Dado que la base de la función exponencial es $e \approx 2.718$ ($e > 1$), a medida que el exponente $x$ tiende a $\infty$, **la función crece sin límite.**

$$\lim_{x\to\infty} e^x = \infty$$

---
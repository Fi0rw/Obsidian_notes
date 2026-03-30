## UML (Unified Model Language)
Para representar visualmente la relación entre clases se usa el **Lenguaje Unificado de Modelado (UML)**. Es el estándar en la industria para comunicar diseños entre programadores, diseñadores, gerentes, etc. 

Un diagrama básico se vería así básicamente dice que `Manzana` se encuentra asociada con el `Barril` y `Naranja` se encuentra asociada con `Canasta`:

~~~txt
┌───────────┐         ┌────────┐
│  Manzana  │─────────│ Barril │
└───────────┘         └────────┘

┌───────────┐        ┌─────────┐
│  Naranja  │────────│ Canasta │
└───────────┘        └─────────┘
~~~

La **Asociación** es la forma más básica de relacionar dos clases. 

## Multiplicidad 
El diagrama básico nos dice que hay una relación, pero no de cuántos objetos participan en ella. Para ello se agrega la **multiplicidad**. 

~~~txt
┌───────────┐  *   go in  1   ┌────────┐
│  Naranja  │─────────────────│ Barril │
└───────────┘                 └────────┘

┌─────────┐   *    go in  1   ┌──────────┐
│ Manzana │───────────────────│  Canasta │
└─────────┘                   └──────────┘
~~~

Se lee (de derecha/izquierda o izquierda/derecha) como: 
- `1`: Exactamente 1 objeto.
- `*`: Muchos objetos (cero o más).
E.g: de izquierda a derecha en 1 (uno) `Barril` van * (muchas) `Naranja`
## What is UML for?
UML es muy útil para: 
- Comunicarse rápidamente en reuniones de equipo.
- Recordar decisiones de diseño propias.
- Documentar relaciones entre clases de forma estándar e intuitiva. 


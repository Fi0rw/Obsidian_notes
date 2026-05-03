## Instrumentos de Medición
Los fenómenos electrónicos son **invisibles a simple vista**. No es posible ver cuántos volts tiene un cable, ni cuántos amperes circulan por él con solo mirarlo. 

Los instrumentos de medición electrónicos existen para hacer visible lo invisible, **traducen magnitudes eléctricas (tensión, corriente, resistencia, frecuencia...) en un valor númerico legible por el operador.** 

> Es importante conocer cómo conectar correctamente cada instrumento tanto como saber leer estos mismos, debido a que una interpretación incorrecta de los valores medidos puede causar accidentes graves (electrocución, incendios, daño de equipos...)

---

## Conceptos Previos Necesarios
Antes de entender los instrumentos es necesario tener claro tres conceptos básicos de los mismos. 

##### 1. Corriente Continua (CC) vs Corriente Alterna (CA)
Cada instrumento específica si es para CC, CA, o ambos. usar un instrumento de CC en un circuito de CA (o viceversa) puede **dañar el instrumento** o dar lecturas incorrectas. 

| Tipo                  | Característica                                                                                   | Ejemplos                          |
| --------------------- | ------------------------------------------------------------------------------------------------ | --------------------------------- |
| **CC** (DC en inglés) | La corriente siempre fluye en el **mismo sentido**. El voltaje es constante.                     | Pilas, baterías, fuentes de PC    |
| **CA** (AC en inglés) | La corriente **invierte su sentido** periódicamente (en Argentina: 50 veces por segundo = 50 Hz) | Red eléctrica domiciliaria (220V) |
##### 2. Conexión en Serie vs Paralelo
~~~txt
SERIE: los componentes se conectan uno tras otro.
       La corriente pasa por todos obligatoriamente.

       ──[A]──[R1]──[R2]──
       
       (el amperímetro A está en serie: la corriente 
        debe pasar por él para seguir el circuito)


PARALELO: los componentes se conectan en los mismos
          dos puntos (comparten terminales).
          
       ──┬──[V]──┬──
         │       │
         └──[R]──┘
         
       (el voltímetro V está en paralelo con R:
        ambos "ven" la misma diferencia de potencial)
~~~

**Regla para recordar**: 
- **Voltímetro** → **Para**lelo → mide tensión **sobre** el componente
- **Amperímetro** → **Serie** → mide corriente que **pasa por** el circuito
- **Óhmetro** → **Para**lelo → mide resistencia **del** componente

##### 3. Polaridad
En corriente continua (CC), la electricidad fluye de un polo negativo (-) a uno positivo (+). Los instrumentos de CC tienen **terminales marcados** con + y -. 
- **Terminal rojo** = positivo (+)
- **Terminal negro** = negativo (-) o tierra (COM)

--- 

## Voltímetro
Mide la **tensión eléctrica** (también llamada diferencia de potencial, voltaje o ddp) entre dos puntos de un circuito. 

> **Unidad**: $\text{Volt } (V)$
> Si el circuito fuera una cañería de agua, la tensión sería la presión del agua. El voltímetro mide cuánta "presión eléctrica" hay entre dos puntos

Se conecta **siempre en PARALELO** con el elemento o circuito que se quiere medir. Se conecta así debido a que necesita "sentir" la misma diferencia de potencial que existe en los extremos del componente. Si se conectara en serie, alteraría completamente el circuito y la lectura sería incorrecta. 

![Voltímetro en circuito imagén](https://www.pcbasic.com/Uploads/files/20250207/b58b156eb07da843b5c3523880a40a03.webp)

Su **símbolo en el circuito** es un circulo con una $V$ dentro. Sus reglas de uso son: 
- Respetar la polaridad en CC (rojo al + del circuito, negro al -)
- Verificar que el instrumento sea el adecuado para CC o CA según corresponda. 
- Si no se conoce el rango aproximado, arrancar siempre desde el rango más alto e ir bajando. 

| Tipo          | Características                                                                                        |
| ------------- | ------------------------------------------------------------------------------------------------------ |
| **Analógico** | Tiene una aguja que se desplaza sobre una escala graduada. Requiere interpretar la escala visualmente. |
| **Digital**   | Muestra el valor directamente en un display numérico. Mayor precisión y más fácil de leer.             |
![IMAGEN VOLTIMETRO EN CIRCUITO](https://www.electricasas.com/wp-content/uploads/voltimetro-como-se-realiza-la-medicion.jpg)

--- 

## Amperímetro
Mide la **corriente eléctrica** que circula por un conductor o circuito.

> **Unidad**: $\text{Ampere } (A)$
> Si la tensión es la presión, la corriente es el caudal de agua que pasa. El amperímetro mide cuántas cargas eléctricas pasan por segundo por ese punto. 
> 

Se conecta **siempre en SERIE** con el circuito. Esto implica tener que cortar o interrumpir el circuito para intercalar el instrumento. Esto es así debido a que se quiere medir la corriente que es la que pasa físicamente por el instrumento.  

![Amperímetro en serie circuito imagén](https://i0.wp.com/www.ingmecafenix.com/wp-content/uploads/2018/08/Amperimetro-de-convencional.webp?resize=683%2C384&ssl=1)

Su símbolo en el circuito es un circulo con la letra $A$ dentro. Sus reglas de uso son: 
- Respetar la polaridad en CC
- **NUNCA conectar en paralelo**, puede quemar el instrumento o causar un cortocircuito. 
- Verificar el rango máximo antes de conectar 
- Respetar si es para CA o CC 

| Tipo                    | Características                                                                                           |
| ----------------------- | --------------------------------------------------------------------------------------------------------- |
| **Analógico**           | Aguja sobre escala. Requiere interrumpir el circuito para instalar.                                       |
| **Digital**             | Display numérico. Más preciso. Igualmente requiere cortar el circuito.                                    |
| **Pinza amperimétrica** | Variante especial: **no requiere cortar el circuito** ([[Instrumentos Electrónicos#Pinza Amperimétrica]]) |

![AMPERIMETRO EN CIRCUITO IMAGEN](https://www.electricasas.com/wp-content/uploads/amp1.jpg)

---

## Óhmetro
Mide la **resistencia eléctrica** de un componente (resistor, bobina, cable, etc.). 

> **Unidad**: $\text{Ohm } (\ohm)$
> La resistencia es la "fricción" o "dificultad" que ofrece un material al paso de la corriente eléctrica. 

Se conecta en **PARALELO** con el elemento resistivo a medir. El óhmetro funciona inyectando su propia pequeña corriente a través del componente para medir cuánto se opone. **El componente a medir no debe estar conectado al circuito mientras se mide**, si el componente está conectado al circuito: 
- Hay otras fuentes de tensión presentes que interfieren con la medición
- El resultado será **incorrecto**
- Se puede **dañar el óhmetro** 

![Ohmetro Imagen](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTc-dIRzSJ39YQhrSJuOSYN2KCWnkXgn2nF2A&s)

El procedimiento correcto sería: 
1. Apagar o desconectar el circuito
2. Retirar físicamente el componente (o al menos desconectar uno de sus extremos del circuito)
3. Conectar el óhmetro en sus terminales
4. Leer el valor

![OHMETRO EN CIRCUITO IMAGEN](https://www.electricasas.com/wp-content/uploads/oh2.jpg)

---

## Variantes Especiales del Óhmetro
##### Megóhmetro (Megger)
- Mide **resistencias de muy alto valor** (del orden de Megaohms o Gigaohms)
- Se usa para verificar el **aislamiento**de cables, motores, tranformadores, etc. 
- Trabaja con tensiones de prueba más altas (500V, 1000V, 2500V) para "forzar" corriente a través de aislamientos. 
- Uso típico: comprobar que el aislamiento de un cable no esté deteriorado. 


##### Telurímetro
- Mide la **resistencia de la toma a tierra** (puesta a tierra, GND)
- Su objetivo es verificar la **equipotencialidad del suelo**, que la conexión a tierra de una instalación eléctrica sea lo suficientemente baja en resistencia para ser segura. 
- Usa 3 o 4 electrodos clavados en el suelo para la medición. 
- Una puesta a tierra deficiente puede ser letal en caso de falla eléctrica.

---

## Otros Instrumentos de Medición

##### Frecuencíometro
- **Mide la frecuencia** de una señal eléctrica
- Unidad: $Hz$
- Uso típico: verificar que la red eléctrica opere a 50Hz (En Argentina), medir frecuencias de osciladores, señales de audio o RF. 
- Existe en versión analógica (aguja) y digital (display)

##### Capacímetro
- **Mide la capacitancia** (capacidad de almacenar carga eléctrica) de condensadores/capacitores. 
- Unidad: Farad ($F$), en la práctica se usan microfarads ($μF$), nanofarads ($nF$) y picofarads ($pF$).
- Muchos multímetros modernos incluyen esta función. 

##### Inductómetro
- **Mide la inductancia** en bobinas y transformadores.
- Unidad: Henry ($H$), en la práctica se utiliza milihenry ($mH$) y microhenry ($μH$).
- A veces se combina con el capacímetro en un instrumento llamado **LCR meter** (mide inductancia, capacitancia y resistencia). 

##### Vatímetro (Medidor de Potencia)
- **Mide la potencia** eléctrica. 
- Unidad: Watt ($W$), en escalas grandes es $MW$ (Megawatt)
- Necesita medir simultáneamente la tensión y corriente para calcular la potencia $P = V \times I$ (en CC)
- Exiswten versiones analógicas y digitales, y también medidores combinados que muestran V y A al mismo tiempo en el mismo display. 


---

## El Multímetro (Tester)
Es un instrumento **todo-en-uno**que reúne en un solo aparato la mayoría de las funciones de los instrumentos individuales vistos. Nombre coloquial: Tester. 
- **Funciones típicas de un multímetro**: 
	- Voltímetro (CC y CA)
	- Amperímetro (CC y CA)
	- Óhmetro
	- Continuidad
	- Test de diodos
	- Test de transistores (hFE)
	- Capacímetro 
	- Termómetro
	- Frecuencímetro

| Borne    | Color | Uso                                                                    |
| -------- | ----- | ---------------------------------------------------------------------- |
| **COM**  | Negro | Siempre conectado al cable negro (negativo / común)                    |
| **VΩmA** | Rojo  | Para medir tensión, resistencia y corrientes bajas                     |
| **10A**  | Rojo  | Para medir corrientes altas (hasta 10A) — borne separado por seguridad |
![IMAGEN MULTIMETRO](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2F4.bp.blogspot.com%2F-obMe9qkHcGE%2FWTcl98Edy5I%2FAAAAAAAADCo%2FcSJ2u2K80B4LomYD1B9uk3yXvm4vDgkcQCLcB%2Fs1600%2FMULTIMETRO%252BCHICO.jpg&f=1&nofb=1&ipt=f94324a3ac459cd3d431d9e2fd2f8f2f68589af448f5c6fb9929886fc1a4aba6)
##### Rangos de medición
Los multímetros tienen distintos **rangos** para cada función. Por ejemplo, para tensión continua pueden tener rangos de 200mV / 2V / 20V / 200V / 600V.

**Regla:** Siempre empezar desde el **rango más alto** e ir bajando hasta obtener una lectura cómoda y precisa. Si se empieza en un rango bajo y el valor supera el máximo, el display mostrará "1" o "OL" (overload = sobrecarga).

![CIRCUITO MULTIMETRO](https://cdn.certicalia.com/images/escaladoHorizontal370px/5127-multimetro.jpg)

--- 

## Pinza Amperimétrica
Es una variante del amperímetro que permite medir corriente **sin cortar el circuito** y **sin contacto directo**con el conductor. Aprovecha el principio de **inducción electromagnética**: todo conductor por el que circula corriente genera un campo magnético a su alrededor. La pinza tiene un sensor que detecta ese campo y circula la corriente correspondiente. 

![IMAGEN PINZA AMPERIMETRICA](https://cdn.certicalia.com/images/escaladoHorizontal370px/5128-pinza-amperimetrica-precio.jpg)

##### Ventajas:
- No requiere interrumpir el circuito
- Mucho más seguro para medir en circuitos energizados
- Ideal para instalaciones en servicio (no se puede apagar)

##### Limitaciones: 
- Solo funciona bien con CA (la inducción requiere campo variable)
- Las versiones para CC son más caras y menos comunes
- El conductor debe pasarse individualmente por la pinza (no funciona con dos conductores juntos, porque los campos se cancelan)

--- 

## Preguntas frecuentes:
> **P: ¿Por qué el óhmetro no puede usarse con el componente en el circuito?**  
> R: Porque el óhmetro inyecta su propia corriente para medir. Si hay otras fuentes de tensión en el circuito, interfieren con esa corriente → lectura incorrecta y posible daño al instrumento.

> **P: ¿Qué pasa si conectás el amperímetro en paralelo?**  
> R: El amperímetro tiene resistencia interna muy baja (casi cero). En paralelo actúa como un cortocircuito → pasa una corriente enorme → quema el fusible del instrumento o se daña el circuito.

> **P: ¿En qué se diferencia el voltímetro analógico del digital?**  
> R: El analógico usa una aguja sobre escala graduada (requiere interpretación visual y puede tener error de paralaje). El digital muestra el valor directamente en un display numérico, con mayor precisión.

> **P: ¿Qué es la pinza amperimétrica y cuál es su ventaja principal?**  
> R: Es un amperímetro que funciona por inducción electromagnética. Su ventaja es que mide la corriente sin necesidad de cortar el circuito ni hacer contacto eléctrico directo con el conductor.
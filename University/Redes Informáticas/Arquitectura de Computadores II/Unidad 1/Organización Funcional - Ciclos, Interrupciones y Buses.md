## Ciclos de Captación y Ejecución
Un programa es un conjunto de instrucciones almacenadas en memoria. La función básica de un computador es ejecutarlas. El proceso más simple tiene dos etapas que se repiten continuamente: 

~~~
INICIO → [Captar instrucción] → [Ejecutar instrucción] → (repetir) → PARADA
~~~

![Ciclo de Captacion y Ejecucion](https://external-content.duckduckgo.com/iu/?u=http%3A%2F%2F3.bp.blogspot.com%2F-TyyJ-moHb0U%2FVRnEb2dtgDI%2FAAAAAAAAAAo%2FQsCKyheHvRY%2Fs1600%2F1.png&f=1&nofb=1&ipt=61efb04b2e1af38dec9c85979e0cc5217d32804b6858d347c7d21abea08065fa)

- **Ciclo de captación (fetch)**: la CPU lee la instrucción de memoria. 
- **Ciclo de ejecución**: la CPU lleva a cabo la acción que indica esa instrucción. 
El ciclo se detiene únicamente si: la máquina se apaga, ocurre un error o se ejecuta una instrucción que lo detiene. 

## PC (Program Counter)
Al inicio de cada ciclo, la CPU necesita saber **de dónde captar la próxima instrucción**. Para esto existe el **PC (Program Counter)**. Es un registro que guarda la dirección de memoria de la siguiente instrucción a ejecutar. 

La CPU **siempre incrementa el PC automáticamente** después de captar cada instrucción, para pasar a la siguiente posición de memoria. 
- E.g: si el `PC=300`, capta la instrucción en la posición `300`, luego el `PC=301`, capta la de `301`, pasa a la `302` y así sucesivamente. 

El **PC (Program Counter)** puede alterarse con instrucciones de salto (`JMP, JNZ, JZ,` etc)

## IR (Instruction Register)
La instrucción captada de memoria no se ejecuta "en el aire": se guarda en el **IR (Instruction Register)**. La instrucción está codificada en binario y específica la acción que debe realizar la CPU. 

Cuando la **CPU decodifica el IR**, la acción puede ser de uno de estos cuatro tipos: 
- **Procesador-Memoria**: transferir datos entre CPU y memoria (read/write).
- **Procesador-E/S**: transferir datos entre CPU y un módulo de E/S. 
- **Procesamiento de datos**: realizar una operación aritmética o lógica. 
- **Control**: alterar la secuencia de ejecución (saltar a otra dirección). 
Una instrucción puede combinar varias de estas acciones.
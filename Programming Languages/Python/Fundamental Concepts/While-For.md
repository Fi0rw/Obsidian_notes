## While Loop
El bucle `while` permite ejecutar algo siempre que la instrucción se cumpla. `while there is something to do: do it` se usa cuando la condición de finalización depende de estado que cambia durante la ejecución y no sabes cuántas iteraciones serán necesarias. Es útil para esperar hasta que se cumpla una condición 

~~~Python
sheep = 0

while sheep <= 9:
	sheep += 1
	print(f"Ship N°{sheep}")
~~~

## For Loop
El bucle `for` se usa cuando conoces de antemano cuántas iteraciones necesitas o cuando iteras directamente sobre una secuencia (listas, rangos, cadenas, diccionarios).
- `range()`: genera una secuencia de enteros perezosamente (no crea una lista completa) que suele usarse en for para iterar un número concreto de veces o sobre índices

~~~Python
sheep = 0

for sheep in range(10):         
	print(f"Sheep N°{sheep}")    
sheep += 1   
~~~


## Break and Continue statement
`continue`: salta inmediatamente a la siguiente iteración del bucle, omitiendo el resto del bloque actual; se usa cuando quieres saltarte procesamiento para casos concretos pero seguir iterando (por ejemplo, ignorar valores inválidos en una lista).

`break`: termina por completo el bucle en el que está; se usa cuando ya se alcanzó el objetivo o se detectó una condición que hace innecesarias más iteraciones (por ejemplo, encontrar un elemento y detener la búsqueda).

~~~Python
# break - example
print("The break instruction:")
for i in range(1, 6):
    if i == 3:
        break
    print("Inside the loop.", i)
print("Outside the loop.")


# continue - example
print("\nThe continue instruction:")
for i in range(1, 6):
    if i == 3:
        continue
    print("Inside the loop.", i)
print("Outside the loop.")
~~~
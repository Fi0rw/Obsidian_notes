Las listas son colecciones ordenadas y mutables en Python, escritas con corchetes: `[1, "a", 3.14]`.
- Indexables (a[0]), start en 0.
- Mutables: append, pop, remove, a[i]=x.
- Heterogéneas: pueden mezclar tipos.
- Útil para almacenar elementos en orden y modificarlos.

~~~Python
numbers = [10, 5, 7, 2, 1]
print("Original list contents:", numbers)  # Printing original list contents.

numbers[0] = 111
print("New list contents: ", numbers)  # Current list contents.
~~~

## The len() function
`len()` devuelve la cantidad de elementos de un objeto iterable o contenedor (longitud)

~~~Python
myList = ["Soy la posicion 0", "Soy la posicion 1", "Soy la posicion 2", "Soy la posicion 3", "soy la posicion 4"]

print(len(myList))

# Output: 5
~~~

## Append() Method
El método `append(x)` añade el elemento x al final de la lista.

~~~Python
a = [1,2]
a.append(3)  # a -> [1,2,3]
~~~ 

## Insert() Method
El método `insert(i,x)` inserta en el indice i, el elemento x, desplazando los elementos a la derecha. Si `i <= 0` lo pone al inicio, si `i >= len(lista)` lo añade al final. 

~~~Python
a = [1, 2]
a.insert(1, 9)  # a -> [1, 9, 2]
~~~
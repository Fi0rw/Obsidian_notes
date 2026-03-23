Python clasifica los valores en **tipos de datos** los cuales determinan qué operaciones pueden realizarse con ellos y cómo se almacenan en memoria. Existen diferentes tipos de datos:
- Es posible comprobar el tipo de dato de una variable utilizando la función `type(variable_name)`
#### Integer (Entero)
Números enteros, sin parte decimal. E.g: **42**, **-7**, etc.
~~~Python
myInteger = 10
print('Integer:', myInteger) 

# Output= Integer: 10
~~~

#### Float (Flotante)
Números de punto flotante (decimales). E.g: **3.14**, **-0.001**, etc.
~~~Python
myFloat = 17.5
print('Float:', myFloat) 

# Output= Float: 17.5
~~~

#### Boolean (Booleano)
Valores lógicos de verdad, E.g: **True**, **False**, etc.
~~~Python
myBoolean = True
print('Boolean:', myBoolean) 

# Output= Boolean: True
~~~

#### String (Cadena)
Cadena de texto, inmutables. E.g: **"hello"**, **"Python"**, etc.
~~~Python
myString = "Hello I'm a String"
print('String:', myString) 

# Output= String: Hello I'm a String
~~~

#### Set (Conjunto)
Una colección ordenada de elementos únicos. E.g: `{0.5, 4, 'Equisde'}`
~~~Python
mySetVariable = {0.17, 80, 'this is a Set variable'}
print('Set:', MySetVariable)

# Output= Set: 0.17, 80, 'this is a Set variable'
~~~

#### Dictionary (Diccionario)
Una colección de pares clave-valor encerrados en llaves. E.g: `{'Nombre': 'John Doe', 'Edad': 28}`

~~~Python
myDictionary = {'Name': 'Tobi', 'Age': 21}
print('Dictionary:', myDictionary) 

# Output= Dictionary: 'Name': 'Tobi', 'Age': 21
~~~

#### Tuple (Tupla)
Una colección ordenada e inmutable, encerrada entre paréntesis. E.g: `('Apple', 4.1, 17)`

~~~Python
myTuple = ('Tobi', 21, 1.60)
print('Tuple:', myTuple)

# Output= Tupla: 'Tobi', 21, 1.60
~~~

#### Range (Rango)
Una secuencia de números, a menudo usada en bucles. E.g: `range(5)`

~~~Python
myRange = range(17)
print('Range:', myRange) 

# Output= Range: range(0, 17)
~~~

#### List (Lista)
Una colección ordenada de elementos que admite diferentes tipos de datos. 

~~~Python
myList = [17, 'Hello World', 3.14, False]
print('List:', myList) 

# Output= List: [17, 'Hello World', 3.14, False]
~~~

#### None (Ninguno)
Un valor especial que representa la ausencia de un valor. 

~~~Python
myNone = None
print('None:', myNone) 
# Output= None: None
~~~
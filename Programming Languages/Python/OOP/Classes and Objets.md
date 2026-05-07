## What are Classes and Objets
En Python, las clases y los objetos trabajan juntos para organizar y gestionar datos. Las clases se construyen para definir comportamientos compartidos, luego se crean los objetos que usan esos comportamientos. 

Para crear una clase, se utiliza `class` seguida del nombre de la clase y dos puntos. Luego, dentro de la clase, es posible añadir un **iniciador** o "**constructor**" junto con cualquier atributo y método. 

Los **atributos** son como variables dentro de una clase y se utilizan para almacenar datos. Los **métodos** son funciones definidas dentro de una clase y son las acciones que los objetos creados con una clase pueden realizar. 

~~~python
class ClassName: 
	def __init__(self, name, age):
		self.name = name
		self.age = age
	
	def simple_method(self): 
		print(self.name.upper())
~~~
- `class ClassName:` está compuesto por la palabra clave `class` para crear la clase, seguida del nombre de la clase `ClassName`
- `def __init__(self, name, age):` es el método especial que se denomina **constructor** o **iniciador**, inicializa los atributos de los objetos que serán creados con la clase. Además de eso, el primer parámetro de `__init__` siempre es una referencia al objeto específico que se está creando o usando. Por convención, este parámetro se llama `self` (aunque es posible usar cualquier otro nombre), este permite acceder a los propios atributos y métodos del objeto. 
- `self.name = name` y `self.age = age` son los atributos que tendrán los objetos. 
- `def sample_method(self):` es el método que cada objeto creado puede llamar. 
- `print(self.name.upper())` es lo que hará el método `sample_method`, en este caso imprime el nombre en mayúsculas. 

--- 

## How to Create Objects
Ejemplo similar de una clase `Dog` y cómo es posible crear objetos a partir de esa clase: 

~~~python
class Dog: 
	def __init__(self, name, age):
		self.name = name
		self.age = age
	
	def bark(self): 
		print(f"{self.name.upper()} says woof woof!")
~~~

Con esta clase `Dog`, es posible crear un objeto. Sintaxis básica para crear un objeto: 

~~~python
object_1 = ClassName(attribute_1, attribute_2)
object_2 = ClassName(attribute_1, attribute_2)
~~~

Es posible también llamar a cualquiera de los métodos definidos en la clase desde cada objeto: 

~~~python
object_1.method_name()
object_2.method_name()
~~~

Ahora procedemos a crear dos perros usando la clase `Dog` como la estructura: 

~~~Python
class Dog: 
	def __init__(self, name, age):
		self.name = name
		self.age = age
		
	def bark(self):
		print(f"{self.name.upper()}  says woof woof! I'm {self.age} years old!")

dog_1 = Dog("Jack", 3)
dog_2 = Dog("Thatcher", 5)

dog_1.bark() #JACK says woof woof! I'm 3 years old!
dog_2.bark() #TATCHER says woof woof! I'm 5 years old!
~~~

En resumen, la diferencia entre una clase y un objeto es que una clase es una platilla/molde o la estructura y un objeto es lo que se crea usando esa plantilla/molde o estructura. 

Ademas, una clase define qué datos y comportamientos debe tener el objeto, y un objeto contiene los datos reales y usa ese comportamiento. Se escribe una clase una vez, y es posible crear muchos objetos a partir de ella, cada uno con datos diferentes. 
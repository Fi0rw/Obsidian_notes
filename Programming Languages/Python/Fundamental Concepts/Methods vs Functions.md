## Method
Un método es un tipo específico de función. Se comporta como una función y parece una función pero difiere en la manera en la que actúa y como se invoca. 
- Técnicamente es una función ligada a un objeto o clase; se define dentro de una clase y se invoca sobre una instancia (o la clase) usando la sintaxis obj.metodo(). Un método puede acceder y modificar el estado del objeto (self) y suele operar sobre los datos de esa instancia.

~~~Python
class Contador:
    def __init__(self):
        self.valor = 0

    def incrementar(self, cantidad=1):   # este es un método
        self.valor += cantidad

c = Contador()
c.incrementar(5)   # invocación del método sobre la instancia
print(c.valor)     # 5
~~~

> Note: Una instancia es un objeto creado a partir de una clase
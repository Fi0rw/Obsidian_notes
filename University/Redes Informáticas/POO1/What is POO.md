La **Programación Orientada a Objetos (POO)** es un paradigma de programación, es decir, un modelo o un estilo de diseño de software que organiza el código alrededor de "**objetos**" en lugar de funciones o lógica estructurada pura. 

Es importante debido a que es el estándar en la industria (usada en lenguajes como **Python, Java, C++ o C#**) porque permite:
- **Reutilización**: Es posible usar el mismo "Molde" (clase) muchas veces. 
- **Mantenimiento**: Es más fácil encontrar errores en un objeto específico que en miles de lineas de código suelto. 
- **Escalabilidad**: Permite construir sistemas complejos de forma modular. 

### El concepto de Clase y Objeto
Si se quisiese representar autos en un sistema:  
- **La Clase**: Es el molde o el plano. Define que un auto debe tener un color, una marca y la capacidad de arrancar. Es la definición. 
- **El Objeto**: Es la instancia real creada con ese molde. E.g: un "Toyota Corolla Azul" o un "Ford Mustang Rojo"

## Pilares de POO
La POO se sostiene sobre cuatro conceptos clave que permiten que el código sea reutilizable y organizado: 
1. **Abstracción**: Consiste en aislar los elementos esenciales de un objeto y ocultar los detalles complejos que no son necesarios para el usuario. 
		E.g: Para conducir un auto, solo se necesita saber usar el volante y los pedales, no es necesario entender como funciona la combustión interna del motor ni nada de ello. 
2. **Encapsulamiento**: Es el "empaquetado" de los datos y los métodos que los manejan dentro de una unidad (la clase), protegiéndolos del acceso externo no autorizado. 
		E.g: Los atributos de un objeto suelen ser privados y solo se modifican a través de métodos específicos. 
3. **Herencia**: Permite que una clase nueva (clase hija) adopte propiedades y métodos de una clase ya existente (clase padre).
		E.g: Es posible tener una clase padre "Vehículo" y clases hijas como "Auto", "Moto" o "Camión". Todos heredan la característica "Moverse", pero cada uno tiene detalles propios.    
4. **Polimorfismo**: Es la capacidad de que diferentes objetos respondan de manera distinta a un mismo mensaje o comando. 
		E.g: Si se da la orden "Moverse" a un objeto "Perro", este caminará; si se la da a un objeto "Pájaro", este "volará". El comando es el mismo, pero la ejecución depende del objeto.  

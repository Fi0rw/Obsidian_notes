#### Variables 
Una variable es **un nombre que hace referencia a un objeto en la memoria**. E.g:

~~~Python
x = 10  # 'x' apunt a un objeto entero (integer) con el valor 10
b = "Site B" # 'b' apunta a un objeto cadena (string) "Site B"
~~~

La misma variable puede apuntar a objetos de distinto tipo durante la ejecución. No hay declaración de tipado previa. Python es un lenguaje de tipado dinámico. 
#### Naming Variables
Las variables deben seguir ciertas reglas al ser declaradas: 
- Los nombres de las variables **solo pueden comenzar con una letra o un guión bajo**, no un número. E.g: `Taza = 2`, `_Taza = 2` 
- Los nombres de las variables **solo pueden contener caracteres alfanuméricos** (`a-z. A-Z, 0-9`) **y guiones bajos** (`_`)
- Los nombres de **las variables son sensibles a mayúsculas**. E.g: `Edad`, `edad` y `EDAD` se consideran variables diferentes. 
- Los nombres de **las variables no pueden ser una de las palabras reservadas de Python** como `if`, `class` o `def` por ejemplo. 

En caso de que alguna regla sea rota, el programa en Python lanzará `SyntaxisError`: 

~~~Python3
line 8
    self.8nombre = "Juan"      
         ^
SyntaxError: invalid decimal literal
~~~


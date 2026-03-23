#### Indentation
En Python la indentación **define bloques**. Un `if`, `for`, `def`, `class` requieren indentación consistente. PEP8 recomienda **4 espacios** por nivel. E.g de indentación:  

~~~Python
if True:
	x = 1
	print(x)
~~~

Error típico: `IndentationError` o `UnboundLocalError` si la indentación cambia el scope.
El condicional permite tomar decisiones. En python los condicionales se declaran con `if`, `elif` y/o `else`

~~~Python
numI = int(input("Ingrese el NumI: "))
numII = int(input("Ingrese el NumII: ")) 

if numI > numII: 
	print("NumI es mayor que NumII")
elif numI < numII: 
	print("NumII es mayor que NumI")
else:
	print("NumI y NumII son iguales!")
~~~
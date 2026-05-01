Aquí tienes notas para copiar en Obsidian: explicaciones claras y ejemplos de código en cada punto.

## ¿Qué es una función?
Una función es un bloque de código reutilizable que realiza una tarea concreta. Recibe argumentos (opcionales), ejecuta instrucciones y suele devolver un resultado.

## Sintaxis básica
- Definición: `def nombre(param1, param2):`
- Llamada: `nombre(arg1, arg2)`

```python
def saludar(nombre):
    """Devuelve un saludo personalizado."""
    return f"Hola, {nombre}!"
```

## Parámetros y valores por defecto
- Puedes dar valores por defecto a parámetros para que sean opcionales.

```python
def potencia(x, n=2):
    """Eleva x a la n (por defecto al cuadrado)."""
    return x ** n
```

## Tipos de argumentos: posicionales y nombrados
- Llamadas posicionales: `f(a, b)`
- Llamadas por nombre: `f(a=1, b=2)` (más legible)

```python
def ejemplo(a, b=10):
    return a + b

ejemplo(3)      # posicional
ejemplo(a=3, b=5)  # nombrado
```

## *args y **kwargs
- `*args` recoge tuplas de argumentos posicionales.
- `**kwargs` recoge diccionarios de argumentos nombrados.

```python
def resumen(*args, **kwargs):
    print("Posicionales:", args)
    print("Nombrados:", kwargs)

resumen(1, 2, x=10, y=20)
```

## Valor de retorno
- `return` devuelve un valor; sin `return` la función devuelve `None`.

```python
def dividir(a, b):
    if b == 0:
        raise ZeroDivisionError("División por cero")
    return a / b
```

## Docstrings y anotaciones de tipo
- Documenta la función con una docstring; las anotaciones son informativas (no obligatorias).

```python
def dividir(a: float, b: float) -> float:
    """Divide a entre b. Lanza ZeroDivisionError si b es 0."""
    if b == 0:
        raise ZeroDivisionError("División por cero")
    return a / b
```

## Funciones anónimas: lambda
- Lambdas son pequeñas funciones en una línea.

```python
doble = lambda x: x * 2
list(map(doble, [1, 2, 3]))  # [2, 4, 6]
```

## Funciones de orden superior
- Reciben o devuelven otras funciones.

```python
def aplicar(f, lista):
    return [f(x) for x in lista]

aplicar(lambda x: x+1, [1, 2, 3])  # [2, 3, 4]
```

## Recursión
- Una función que se llama a sí misma; útil para definiciones repetitivas.

```python
def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)
```

## Generadores (yield)
- `yield` crea un iterador pausable, eficiente para secuencias grandes.

```python
def contador(n):
    i = 0
    while i < n:
        yield i
        i += 1

for c in contador(3):
    print(c)  # 0 1 2
```

## Decoradores (básico)
- Permiten envolver comportamiento antes/después de una función.

```python
def timer(f):
    import time
    def wrapper(*args, **kwargs):
        t0 = time.perf_counter()
        result = f(*args, **kwargs)
        t1 = time.perf_counter()
        print(f"Duración: {t1 - t0:.6f}s")
        return result
    return wrapper

@timer
def suma(a, b):
    return a + b
```


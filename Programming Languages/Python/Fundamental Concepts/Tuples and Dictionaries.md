## Tuplas
Una tupla es una secuencia ordenada e inmutable de elementos. Se usan cuando no quieres que los datos cambien.

```python
# Creación
t = (1, 2, 3)
t2 = 4, 5, 6      # también válido
empty = ()

# Acceso
t[0]    # 1
t[-1]   # 3
len(t)  # 3

# Desempaquetado
a, b, c = t

# Inmutabilidad
# t[0] = 10  -> TypeError

# Métodos (limitados)
t.index(2)   # 1
t.count(2)   # veces que aparece 2
```

Usos comunes: devolver múltiples valores desde una función, claves inmutables (p. ej. coordenadas), iteración segura.

## Diccionarios
Un diccionario es una colección mutable de pares clave:valor, sin orden garantizado (en Python 3.7+ mantiene orden de inserción).

```python
# Creación
d = {"a": 1, "b": 2}
d2 = dict(c=3, d=4)
empty = {}

# Acceso y asignación
d["a"]        # 1
d["e"] = 5    # añade o actualiza

# Obtener con valor por defecto
d.get("z", 0)  # 0 si no existe

# Eliminación
d.pop("a")     # elimina y devuelve el valor
del d["b"]

# Iteración
for k in d:           # claves
    print(k, d[k])

for k, v in d.items():  # claves y valores
    print(k, v)

# Operaciones útiles
keys = list(d.keys())
vals = list(d.values())
pairs = list(d.items())

# Comprensión de diccionarios
squares = {x: x*x for x in range(5)}  # {0:0, 1:1, ...}
```

Diferencias clave:
- Mutabilidad: tupla inmutable; diccionario mutable.
- Acceso: tuplas por índice; diccionarios por clave.
- Uso: tuplas para colecciones fijas/ordenadas; diccionarios para mapeos clave→valor.

## Ejemplos prácticos
```python
# Devolver múltiples valores con tupla
def div_mod(a, b):
    return a // b, a % b

q, r = div_mod(7, 3)  # q=2, r=1

# Agrupar datos por clave con diccionario
people = [("Alice", 30), ("Bob", 25), ("Alice", 31)]
by_name = {}
for name, age in people:
    by_name.setdefault(name, []).append(age)
# {'Alice':[30,31], 'Bob':[25]}
```

- Usar tuplas para datos inmutables y cuando se quiera que sean hashables (p. ej. claves en sets o dicts).
- Usar diccionarios para búsquedas por clave y agrupaciones dinámicas.
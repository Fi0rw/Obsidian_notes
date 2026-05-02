## ¿Qué son las excepciones?
Las excepciones son objetos que representan errores o condiciones excepcionales ocurridas en tiempo de ejecución. Cuando ocurre una excepción, la ejecución normal del programa se interrumpe y Python busca un manejador (handler) adecuado; si no lo encuentra, el programa termina mostrando un traceback.

## Jerarquía y excepciones comunes
Todas las excepciones heredan de BaseException; la mayoría de las que se usan derivan de Exception. Algunas excepciones comunes son:
- ValueError: valor no válido.
- TypeError: tipo incorrecto.
- KeyError: clave ausente en un dict.
- IndexError: índice fuera de rango.
- ZeroDivisionError: división por cero.
- FileNotFoundError: archivo no encontrado.
- IOError / OSError: errores de E/S y del sistema.
- AttributeError: acceso a un atributo inexistente.
- StopIteration: fin de un iterador.
Python 3.11 introdujo ExceptionGroup para agrupar múltiples excepciones en concurrencia/async; es útil en contextos concurrentes.

## Capturar excepciones: try / except
La forma básica para interceptar errores y seguir ejecutando es usar try/except.

```python
try:
    x = int(input("Número: "))
    print(10 / x)
except ValueError:
    print("Debes introducir un número entero.")
except ZeroDivisionError:
    print("No se puede dividir entre cero.")
```

Se pueden capturar varias excepciones en una sola cláusula:

```python
try:
    ...
except (ValueError, TypeError) as e:
    print("Error de entrada:", e)
```

No conviene usar `except:` a secas porque atrapa BaseException (incluye KeyboardInterrupt, SystemExit). Si se necesita atrapar todas las excepciones derivadas de Exception, es mejor usar `except Exception:`.

## else y finally
- `else` se ejecuta si no se lanzó ninguna excepción en el bloque try.  
- `finally` se ejecuta siempre, ocurra o no una excepción (útil para liberar recursos).

```python
try:
    f = open("datos.txt")
    data = f.read()
except FileNotFoundError:
    print("Falta el archivo.")
else:
    print("Archivo leído correctamente:", len(data), "bytes")
finally:
    try:
        f.close()
    except UnboundLocalError:
        pass
```

El patrón moderno recomienda `with` para gestionar recursos y evitar muchos usos de `finally`.

## Lanzar excepciones: raise
Se lanza una excepción con `raise`. Esto es útil para validar entradas o propagar errores.

```python
def dividir(a, b):
    if b == 0:
        raise ZeroDivisionError("b no puede ser 0")
    return a / b
```

Para relanzar la excepción actual dentro de un except se usa `raise` sin argumentos:

```python
try:
    ...
except SomeError:
    # hacer algo
    raise  # relanza la excepción original
```

## Encadenamiento de excepciones
Se puede encadenar una excepción nueva preservando la original con `raise ... from ...`:

```python
def cargar_config(path):
    try:
        with open(path) as f:
            return json.load(f)
    except FileNotFoundError as e:
        raise RuntimeError("No se pudo cargar la configuración") from e
```

El traceback mostrará ambas excepciones, lo que facilita el diagnóstico.

## Excepciones personalizadas
Se definen clases que heredan de Exception para errores de dominio específico.

```python
class MiError(Exception):
    """Error específico de la aplicación."""
    pass

def proceso(x):
    if x < 0:
        raise MiError("x debe ser no negativo")
```

Es posible añadir atributos o métodos para proporcionar más contexto en la excepción.

## Buenas prácticas al manejar excepciones
- Atrapar solo las excepciones que se pueden manejar con sentido; no usar `except Exception:` como parche general.  
- No usar excepciones para control del flujo normal.  
- Proporcionar mensajes claros y contextualizados al lanzar excepciones.  
- Limpiar recursos con `with` o `finally`.  
- Validar entradas antes de operar para evitar excepciones evitables.  
- En librerías, lanzar excepciones específicas de dominio para que los usuarios puedan capturarlas fácilmente.  
- Evitar ocultar la excepción original; si se transforma la excepción, encadenarla con `from`.

## Ejemplos prácticos completos

1) Manejo de entrada robusto:
```python
def pedir_entero(prompt="Introduce un entero: "):
    while True:
        try:
            return int(input(prompt))
        except ValueError:
            print("Valor inválido, inténtalo de nuevo.")
```

2) Lectura de archivo con manejo y reintentos:
```python
import time

def leer_archivo_con_reintentos(path, reintentos=3, delay=1.0):
    for intento in range(1, reintentos + 1):
        try:
            with open(path, "r", encoding="utf-8") as f:
                return f.read()
        except FileNotFoundError:
            if intento == reintentos:
                raise
            time.sleep(delay)
```

3) Agrupación/registro de errores múltiples:
```python
errors = []
for item in lista:
    try:
        procesar(item)
    except Exception as e:
        errors.append((item, e))

if errors:
    for item, exc in errors:
        print("Error en", item, ":", exc)
```

4) Uso de contextlib.suppress para ignorar errores concretos:
```python
from contextlib import suppress
with suppress(FileNotFoundError):
    os.remove("temporal.txt")  # no falla si no existe
```

5) Reemplazar excepciones de bajo nivel por errores de dominio:
```python
class DatabaseError(Exception):
    pass

def get_user(db, user_id):
    try:
        return db.query_user(user_id)
    except TimeoutError as e:
        raise DatabaseError("Timeout accediendo a la base de datos") from e
```

## Diagnóstico: leer tracebacks
El traceback muestra la cadena de llamadas hasta el punto donde ocurrió la excepción y la excepción final con su mensaje. El encadenamiento y mensajes claros mejoran la trazabilidad del error.

## Excepciones y concurrencia
En código concurrente (threads, asyncio), las excepciones pueden agruparse o propagarse de forma diferente; en asyncio es habitual capturar excepciones en tareas y registrarlas. Python 3.11 introdujo ExceptionGroup para manejar múltiples excepciones simultáneas de forma estructurada.

## Resumen
- Usar try/except para manejar errores previsibles.  
- Emplear finally o with para liberar recursos.  
- Levantar excepciones claras y específicas.  
- Encadenar excepciones cuando se transforme un error.  
- Evitar atrapar más de lo necesario para no ocultar problemas.
#### Get-Help Command
- El comando `Get-Help` se utiliza para obtener información sobre cmdlets, funciones, scripts y conceptos en PowerShell.
- Microsoft le añade una característica al comando llamada **updatable help** donde PowerShell puede descargar información actualizada, modificada y expandida del comando `Get-Help` desde internet. Esto mediante el comando `Update-Help`
	- Es posible descargar una copia del **updatable help** con el comando `Save-Help` y actualizarla con el comando `Update-Help -Souce [path]`
	- Algunas cosas a tener en cuenta de `Update-Help` es que puede generar errores al ejecutarse sin privilegios de administrador. Por otro lado, a veces, la ayuda en diferentes idiomas no se encuentra disponible para todos los módulos, es posible forzar la descarga en inglés: `Update-Help -UICulture en-US -Force`
- La diferencia entre `Help` y `Get-Help` es que este ultimo es un cmdlet puro, escupe toda la información directamente dentro de la consola. En cambio `Help` es una función que envuelve Get-Help y le añade paginación (más comodo para leer documentos largos), es posible salir de la paginación con la tecla **Q**. 

#### Get-Help Parameters
- `Get-Help` (o `Help`) tiene como otros comandos, variedad de parámetros. Entre ellos: 
	1. `-Name` Que se utiliza para específicar el nombre del comando el cual obtener ayuda. 
- **Wildcards**: los wildcards son caracteres que se utilizan para representar uno o varios caracteres en una búsqueda. En PowerShell hay dos tipos comunes: 
	1. `* (asteriscos)`: representan cualquier número de caracteres.
	2. `? (signo de interrogación)`: representa un solo caracter.
- E.g: 

~~~PowerShell
Para obtener ayuda para todos los comandos que empiecen con Get:
PS C:\Windows\System32> Get-Help -Name Get-* 

Para obtener ayuda para todos los comandos que se ajusten con ese patron 
PS C:\Windows\System32> Get-Help -Name Get-Task?

Busca todos los comandos/cmdlets que contengan la palabra event
PS C:\Windows\System32> Get-Help *event*
~~~

- `Get-Help` no siempre muestra todo por defecto para no abrumarte. Vale la pena que agregues estos tres parámetros fundamentales a tu lista:

| Parámetro   | Propósito                                                                                |
| ----------- | ---------------------------------------------------------------------------------------- |
| `-Examples` | Muestra solo los ejemplos de uso.                                                        |
| `-Detailed` | Muestra la ayuda técnica, incluyendo explicaciones de los parámetros.                    |
| `-Full`     | Muestra **absolutamente todo**: ejemplos, parámetros, notas técnicas y tipos de objetos. |
| `-Online`   | Abre la documentación más reciente en tu navegador web                                   |

#### Get-Help Syntax
- El comando `Get-Help` al utilizarse muestra **SYNTAX**. Es el conjunto de reglas formales que definen la estructura de una instrucción valida para que el **Parser (analizador sintáctico)** del motor de ejecución pueda interpretarla y procesarla. En `Get-Help` es la representación formal de cómo el _motor de PowerShell_ espera recibir los parámetros y argumentos para un cmdlet determinado.
- **Diccionario de símbolos**: 
	- `[ ]` (corchetes externos) **opcional**: todo lo que está dentro puede omitirse. 
	- `<>` (paréntesis angulares) **tipo de dato**: Indica el **tipo de dato esperado**, no el valor literal.
	- `[-Nombre]` (nombre en corchetes) **posicional**: puede omitirse el nombre del parámetro. 
	- `,` (comas) **colección**: Indica que se puede pasar una lista de valores (E.g: `string[]`).  
- E.g de **SYNTAX** ejecutando `Get-Help -Name Stop-Service`: 

~~~
PS C:\Users\Yw4rf\Documents\Obsidian> Get-Help -Name Stop-Service

NAME
    Stop-Service

SYNTAX
    Stop-Service [-InputObject] <ServiceController[]> [-Force] [-NoWait] [-PassThru] [-Include <string[]>] [-Exclude
    <string[]>] [-WhatIf] [-Confirm] [<CommonParameters>]

    Stop-Service [-Name] <string[]> [-Force] [-NoWait] [-PassThru] [-Include <string[]>] [-Exclude <string[]>]
    [-WhatIf] [-Confirm] [<CommonParameters>]

    Stop-Service -DisplayName <string[]> [-Force] [-NoWait] [-PassThru] [-Include <string[]>] [-Exclude <string[]>]
    [-WhatIf] [-Confirm] [<CommonParameters>]
~~~

Es posible observar: 
- `[-Name] <string[]>`:
	- Como `[-Name]` Se encuentra en corchetes, es **posicional**. Es posible escribir `Stop-Service -Name <string[]>` o `Stop-Service <string[]>`
	- Como `<string[]>` **NO** está entre corchetes, el **valor**es obligatorio si se usa **ese bloque de SYNTAX** (el segundo), el `[]` significa que es posible detener varios servicios a la vez separandolos por comas.  
	- `[-PassTruh]` Todo el parámetro está entre corchetes, por lo tanto es **opcional**. No tiene `<>` lo que significa que es un **Switch** (un interruptor, o se añade o no se añade, no se necesita un valor).

- **Parameter Sets**: Un parameter set es una combinación válida y mutuamente exclusiva de parámetros. Cada línea bajo SYNTAX es un set distinto.
	- **No es posible mezclar parámetros de diferentes conjuntos**. 
		- Si se utiliza un parámetro que pertenece únicamente al "Conjunto A", el motor de PowerShell invalidará cualquier intento de utilizar parámetros exclusivamente al "Conjunto B".
	- **Default Parameter Set**: Si no se específican parámetros que identifiquen un conjunto único, el motor intentará ejecutar el conjunto definido como predeterminado en la metadata del cmdlet.
#### Shortcuts Parameters 
- Es posible utilizar shortcuts para los nombres de los parámetros (algo parecido a los [[Aliases]], que son nombres cortos para los comandos). PowerShell no fuerza a utilizar los nombres de los parámetros completos. Hay tres maneras de utilizar shortcuts: 
	- **Truncating Parameter Names**: En lugar de escribir `-ComputerName` es posibe escribir `-comp`. La regla es que PowerShell necesuta tener las suficientes letras para poder identificar qué comando es en específico.
	- **Using Parameter Name Aliases (Common Parameters)**: Los parámetros también tienen sus propios aliases, aunque no son mostrados en el archivo de ayuda, lo cual hace que sean difíciles de encontrar. Para encontrarlos, se utiliza este comando `(Get-Command [COMMAND] | select -Expand parameters).[PARAMETER].aliases` 
	- **Positional Parameters**: Es posible ver los parámetros posicionales en los archivos de ayuda dentro de la **SYNTAX**. Estos simplemente esperan un valor según su posición en el comando. 
		- E.g: Es posible saltarse el escribir el nombre del parámetro posicional `-Path` y `-Name` en el comando `New-Item`: `New-Item "C:\Carpeta" "NombreDeArchivo.txt"`

~~~PowerShell
Get-Help New-Item

SYNTAX
   [-Path] <String[]>
   [-Name] <String>
   [-ItemType <String>]
   [-Value <Object>]
   [-Force]
   [-Credential <PSCredential>]
   [-WhatIf]
   [-Confirm]
   [<CommonParameters>]
~~~

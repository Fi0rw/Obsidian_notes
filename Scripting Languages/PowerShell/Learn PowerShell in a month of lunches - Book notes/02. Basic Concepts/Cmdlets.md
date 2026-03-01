#### What are cmdlets?
- Una **cmdlet** es un comando ligero y específico diseñado para realizar una tarea particular dentro de PowerShell. Estos existen solo dentro de PowerShell y están escritos tanto en el lenguaje **.NET Core** como **C#**. 
- Los cmdlets siguen una convención de **nomenclatura basada en Verb-Noun** (Verbo-Sustantivo) lo que facilita la comprensión de su funcionalidad. E.g: `Get-Process` (Obtener-Procesos), `Set-Content` (Establecer-Contenido).
	- La palabra se pronuncia como "**command-let**" y es un término exclusivo de PowerShell.
	- Los cmdlets manejan **objetos**, lo que significa que la salida de un cmdlet puede ser utilizada como entrada para otro cmdlet, mediante **pipelines**. E.g: `Get-Process | Where-Object {$_.CPU -gt 100}` (obtiene procesos y filtra aquellos que utilizan más de 100 unidades de CPU)

#### This are not cmdlets 
- Las **funciones** son similares a los cmdlets pero en lugar de ser escritos en lenguajes como .NET, las funciones son escritas en el propio **lenguaje de scripting de PowerShell**.
	- Los archivos `.ps1` son scripts escritos con el lenguaje de scripting de PowerShell.
- Una **aplicación** es cualquier tipo de ejecutable externo, incluyendo utilidades de la linea de comandos como `ping o ipconfig` 
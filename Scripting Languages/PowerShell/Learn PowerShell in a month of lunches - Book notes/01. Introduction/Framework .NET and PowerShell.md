#### What is .NET Framework? 
- .NET es un framework de desarrollo de software creado por Microsoft, diseñado para la creación de aplicaciones que pueden ejecutarse en cualquier sistema operativo compatible.
	- Proporciona un entorno de ejecución, biblioteca de clases y herramientas de desarrollo para simplificar la creación, prueba y mantenimiento de software. 

#### PowerShell and .NET Framework
- PowerShell se basa en el Framework .NET. 
	1. PowerShell ejecuta su código sobre el **CLR (Common Language Runtime)**, es el motor que permite que los programas escritos en diferentes lenguajes de programación se ejecuten en un entorno unificado, facilitando así que PowerShell aproveche las capacidades de .NET sin necesidad de recompilar cada vez.
	2. **Acceso a clases y métodos de .NET**: PowerShell puede instanciar y utilizar clases directamente de las bibliotecas de .NET. También puede invocar métodos de estas clases dentro de los scripts PowerShell. 
	3. **Objetos Nativos**: En PowerShell casi todo retorna como objetos, lo que proviene del manejo de datos del .NET framework. 
	4. **Módulos y Cmdlets**: Los desarrolladores pueden crear cmdlets personalizados que utilizan las bibliotecas de .NET para extender las capacidades de PowerShell, integrando lógica de negocio o interactuando con APIs externas.
	5. **Multiplataforma**: Con la llegada de .NET Core, PowerShell se volvió más versátil permitiendo su uso en sistemas operativos como MacOS y Linux, además de Windows, ampliando su aplicación en entornos diversos.
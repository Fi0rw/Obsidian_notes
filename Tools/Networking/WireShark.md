# 🦈 Wireshark Blue Team Cheatsheet
> **Guía rápida para análisis forense de red y respuesta a incidentes.**

---
## 1. Operadores Lógicos y Sintaxis
Para combinar filtros y aislar el tráfico malicioso.

| Operador   | Alternativa | Descripción                       | Ejemplo                      |        |      |
| :--------- | :---------- | :-------------------------------- | :--------------------------- | ------ | ---- |
| `&&`       | `and`       | Ambas condiciones deben cumplirse | `ip.addr == 10.0.0.1 && tcp` |        |      |
| `\|\|`     | `or`        | Al menos una condición se cumple  | `http                        |        | dns` |
| `!`        | `not`       | Excluye la condición              | `!arp`                       |        |      |
| `==`       | `eq`        | Igual a                           | `tcp.port == 443`            |        |      |
| `!=`       | `ne`        | No es igual a                     | `ip.src != 192.168.1.1`      |        |      |
| `contains` | -           | Busca una cadena (Case Sensitive) | `http contains "password"`   |        |      |
| `matches`  | -           | Busca usando Regex                | `http.host matches "\.(com   | net)"` |      |

---

## 2. Filtros de Red (Capa 3)
Ideal para identificar hosts sospechosos y escaneos.

* **Dirección específica (Origen o Destino):** `ip.addr == 10.10.10.5`
* **Rango de subred (CIDR):** `ip.addr == 192.168.1.0/24`
* **Filtrar solo Origen o solo Destino:** `ip.src == 1.2.3.4` / `ip.dst == 8.8.8.8`
* **Tráfico ICMP (Ping/Errores):** `icmp`
* **TTL bajo (Posible Traceroute o evasión):** `ip.ttl < 10`

---

## 3. Capa de Transporte (TCP/UDP)
Para detectar escaneos de puertos y sesiones C2 (Command & Control).

* **Puerto específico:** `tcp.port == 80` o `udp.port == 53`
* **Rango de puertos:** `tcp.port >= 1024 and tcp.port <= 65535`
* **Buscando el SYN (Inicio de conexión):** `tcp.flags.syn == 1 && tcp.flags.ack == 0`
* **Conexiones rechazadas/cerradas:** `tcp.flags.reset == 1`
* **Análisis de retransmisiones (Problemas de red):** `tcp.analysis.retransmission`
* **Handshake de TLS (SNI/Certificados):** `tls.handshake.extensions_server_name`

---

## 4. Filtros de Aplicación (Threat Hunting)

### 🌐 HTTP (Tráfico en claro)
* **Métodos específicos (POST es clave para exfiltración):** `http.request.method == "POST"`
* **Filtrar por User-Agent (Herramientas de hacking):** `http.user_agent contains "sqlmap" or http.user_agent contains "nmap"`
* **Descarga de archivos sospechosos:** `http.downloaded_file_extension == "exe"` o `http.content_type contains "application"`
* **Códigos de error del servidor:** `http.response.code >= 400`

### 🔍 DNS (Exfiltración y Túneles)
* **Consultas por un dominio específico:** `dns.qry.name contains "malware"`
* **Consultas de tipo TXT (Común en túneles DNS):** `dns.qry.type == 16`
* **Respuestas con TTL muy bajo:** `dns.resp.ttl < 60`

### 📂 SMB/FTP (Movimiento Lateral)
* **Acceso a carpetas compartidas:** `smb2.filename`
* **Comandos FTP:** `ftp.command == "USER"` o `ftp.command == "PASS"`

---

## 5. Técnicas de Análisis Forense

### Seguir el flujo (Stream)
Para ver la conversación completa entre dos puntos:
1. Click derecho sobre un paquete.
2. **Follow** -> **TCP Stream** (o HTTP/TLS Stream).
> *Útil para ver contraseñas en texto plano, comandos ejecutados en una shell reversa o contenido de una web.*

### Extraer Archivos (Export Objects)
Si un atacante descargó un malware vía HTTP o SMB:
1. `File` -> `Export Objects` -> Selecciona el protocolo (HTTP, SMB, etc.).
2. Selecciona el archivo y haz click en `Save`.

### Estadísticas Rápidas
* **Endpoints:** `Statistics` -> `Endpoints`. (Para ver quién está hablando más).
* **Conversations:** `Statistics` -> `Conversations`. (Para ver quién habla con quién).
* **Protocol Hierarchy:** `Statistics` -> `Protocol Hierarchy`. (Para ver si hay protocolos inusuales como IRC o BitTorrent).

---

## 6. Atajos Rápidos

| Atajo                      | Función                                                             |
| :------------------------- | :------------------------------------------------------------------ |
| **Ctrl + F**               | Buscar paquetes por texto, hex o valor.                             |
| **Ctrl + N**               | Ir al siguiente paquete marcado.                                    |
| **Ctrl + M**               | Marcar/Desmarcar paquete.                                           |
| **Ctrl + Shift + O**       | Abrir un nuevo archivo de captura.                                  |
| **Ctrl + Alt + Shift + T** | Cambiar formato de tiempo (Ej: de "segundos desde inicio" a "UTC"). |

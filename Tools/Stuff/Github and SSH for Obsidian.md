### 1. How to Configure Github and SSH 

##### Using Git
- `git -v` o `git --version`
	- verificar la versión de git o si está instalado en el sistema. 
- `git config --global user.name "[USERNAME]"`
- `git config --global user.email "[EMAIL]"`
- `git config --global init.defaultBranch main`
	- renombra la rama a main por default. 
##### Create public and private keys with ssh-keygen
- `ssh-keygen -t ed25519 -C "[comment]"`
	- `-t`: específica el tipo de clave (**rsa, ed25519, ecdsa**), la más segura y eficiente en sistemas modernos es la de **ed25519** 
	- `-b`: define el tamaño de la clave en bits (**solo para rsa**)
	- `-C`: añade un comentario, útil para identificar la clave
	- `-f`: específica un nombre de archivo personalizado para las claves 
##### Verify keys security and permissions in Linux
- `ls -l ~/.ssh`
	- `chmod 700 ~/.ssh`: modifica los permisos para que el directorio `.ssh/` sea solo accesible por el owner (**lectura/ejecución**).
	- `chmod 600 ~/.ssh/id_ed25519`: modifica los permisos para que la clave privada solo tenga los permisos de **read/write** por el owner; es crítico para que ssh no la rechace por insegura.
	- `chmod 644 ~/.ssh/id_ed25519.pub`: modifica los permisos de la clave pública para que tenga permisos de **read** para cualquiera del sistema (no es un archivo crítico la clave pública).
- Esto debido a que SSH se niega a usar una clave privada si otros usuarios pueden leerla.

#####  Key deployment and ssh-agent
- `eval "$(ssh-agent -s)"` lanza el agente y exporta las variables de entorno en la shell actual para que `ssh`/`git` puedan comunicarse con él.
	- **ssh-agent**: es un proceso que actúa como "llavero" en memoria, guarda claves desencriptadas para que no haya que escribir la passphrase cada vez. 
- `ssh-add ~/.ssh/id_ed25519` envía la clave privada al `ssh-agent`. Si la clave tiene passphrase, pedirá la passphrase una vez.
- Control Commands: 
	- `ssh-add -l` → lista las claves actualmente cargadas en el agente (muestra fingerprints).
	- `ssh-add -D` → elimina todas las claves del agente (útil para logout manual).

##### Copy public key and upload to Github
- `cat ~/.ssh/id_ed25519.pub` --> copiar
	- Entrar a cuenta de Github.
	- _Settings_ → _SSH and GPG keys_ → _New SSH key_.
	- En _Title_ algo descriptivo 
	- En _Key_ pegar la clave pública.
	- Guardar.
GitHub almacena la clave pública y la asocia a tu cuenta. A partir de ese momento, cualquier conexión SSH firmada con la correspondiente clave privada se considerará autorizada para actuar como tu usuario de GitHub (según los permisos que tengas en cada repo). La clave pública no permite deducir la privada. Si alguna clave se ve comprometida, en GitHub es posible eliminarla (revocarla) y crear otra.

##### Change remote local repo to use SSH
- `git remote set-url origin [SSH-URL-Github-Repo]`
	- actualiza la configuración del remote `origin` guardada en `.git/config` del repo local. Reemplaza la URL HTTPS por la URL SSH (`git@github.com:usuario/repo.git`).
	- Cuando Git necesite autenticarse usará `ssh` en lugar de HTTPS.
- `git remote -v` comando para verificar. 
Usar SSH elimina la necesidad de tokens y permite autenticación basada en clave, más cómoda para workflows frecuentes.

##### SSH connection test with Github 
- `ssh -t git@github.com` Inicia una conexión SSH al servidor `git@github.com` y solicita una sesión de prueba. Tras la verificación de la clave, GitHub responde con un mensaje identificando tu usuario (no abre un shell; GitHub rechaza shells interactivos y solo devuelve un banner de confirmación).
- Primera vez: pedirá confirmar la huella digital del host (known_hosts):
```
The authenticity of host 'github.com (IP)' can't be established.
RSA key fingerprint is SHA256:...
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```
- Al responder `yes` agrega `github.com` a `~/.ssh/known_hosts` para futuras comprobaciones de integridad del servidor.

##### Git push using SSH
- `git push -u origin main` 
	- `git push` envía tus commits locales a la rama `main` del remote `origin`. `-u` (o `--set-upstream`) establece una relación upstream entre tu rama local `main` y `origin/main`, lo que permite usar `git push` y `git pull` sin argumentos en el futuro.
- Dado que el remote está configurado como `git@github.com:...`, la autenticación se hará por SSH (usando la clave que cargada en el agente y la pública registrada en GitHub).

--- 

### 3. Using Git to commit Notes

##### Look what change - Git Status
- `git status` 
	- Compara el último commit, con el working directory actual. A su vez, detecta archivos nuevos, modificados y archivos eliminados.
``` 
On branch main
Changes not staged for commit:
  modified: Operating Systems/Windows/AD/Kerberos.md

Untracked files:
  Operating Systems/Windows/AD/LDAP.md
```
##### Decide what to upload - Staging  
- `git add .` 
	- copia el estado actual de los archivos al **staging area**, no crea un commit, no sube nada, solo prepara. 
- `git add Operating\ Systems/Windows/AD/Kerberos.md` 
	- lo mismo que el anterior pero para un archivo en específico.
##### Create the commit - Git Commit
- `git commit -m "[commit-title]"`
	- Git toma el **staging area**, calcula hashes, crea un objeto commit. El commit apunta al commit anterior, a un árbol de archivos. El historial se vuelve inmutable (que no puede ni se puede cambiar). Básicamente no se puede modificar sin dejar rastro.
	
##### Upload to Github - Git Push
- `git push`
	- Envia los commits locales que Github no tiene. Manda los objetos commits nuevos.

##### Utils
- `git log` o  `git log --oneline`  
	- para mirar el historial 
- `git diff` 
	- para mirar cambios entre versiones
- `git diff HEAD~1 HEAD` 
	- para mirar cambios entre commits
- `git restore archive.md`
	- en caso de haber agregado o haber hecho un cambió que no se debía en un archivo, funcionará siempre que no se haya hecho commit.


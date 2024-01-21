# Ubuntu-Server
## Instalación
### Particionado
1. Formatear el disco
`reformat`
2. Añadir particiones
`Add GPT Partition`
```
Size (max 0.0G):  [2G]
Format:           [swap]
Mount:            [/]
```
```
Size (max 0.0G):  []
Format:           [btrfs]
Mount:            [/]
```
### Instalar servidor OpenSSH
```
[x]    Install OpenSSH Server
```
## Iniciar sesion SSH vía [PowerShell](https://learn.microsoft.com/es-es/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.4)
```
ssh user@192.168.1.10
```
## Contraseña para usuario `root`
### Permitir que el usuario `root` use contraseña propia
```
sudo nano /etc/ssh/sshd_config
```
Cambiar `#PermitRootLogin prohibit-password`

Por `PermitRootLogin yes`
### Añadir contraseña a usuario `root`
```
sudo passwd root
```
```
sudo systemctl restart sshd
```
## Autentificación por llave SSH
### Creación de la llave SSH
```
su -
```
```
ssh-keygen -t ed25519
```
```
ssh-copy-id root@192.168.1.10
```
### Importación de la llave SSH
#### Mediante [Posh-SSH](https://github.com/LMKHowTO/Posh-SSH_Install) (RECOMENDADO)
```
Get-SCPItem -Destination "C:\Users\Administrador\.ssh" -Path "/root/.ssh/id_ed25519" -PathType File -ComputerName 192.168.1.10 -Credential root
```
#### Mediante WinSCP
1. Descargar e instalar [WinSCP](https://winscp.net/eng/download.php)
2. Iniciar sesion en nuestro servidor con el usuario `root` y la contraseña que anteriorimente le configuramos
3. Click en la barra de direcciones y escribimos `/root/.ssh`
4. copiar el archivo `ed25519` del directorio `/root/.ssh` a `C:\Users\Administrador\.ssh`
### Bloquear acceso mediante contraseña a SSH
```
nano /etc/ssh/sshd_config
```
Cambiar `#PubkeyAuthentication yes`

Por `PubkeyAuthentication yes`

Cambiar `#PasswordAuthentication yes`

Por `PasswordAuthentication no`
```
nano /etc/ssh/sshd_config.d/50-cloud-init.conf
```
Cambiar `PasswordAuthentication yes`

Por `PasswordAuthentication no`
### Reiniciar servicio SSH
```
systemctl restart sshd
```


























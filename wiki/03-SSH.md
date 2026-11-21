# 03 - Aseguramiento del acceso remoto SSH, SELinux y cortafuegos

## 1. Arquitectura y requisitos

Se configuró OpenSSH para aceptar conexiones en el puerto TCP personalizado `2222`. El servidor se identifica como `srv-bueno`, con el usuario `estudiantes` y el nombre de conexión `ssh.bueno.lab`.

| Parámetro | Valor |
|---|---|
| Servicio | `sshd.service` |
| Archivo de configuración | `/etc/ssh/sshd_config` |
| Puerto SSH | `2222/TCP` |
| Usuario remoto | `estudiantes` |
| Nombre de conexión | `ssh.bueno.lab` |
| Hostname | `srv-bueno` |

## 2. Despliegue y configuración

### 2.1 Cambio de puerto de escucha en OpenSSH

Antes de modificar el archivo de configuración se guardó una copia de respaldo:

```bash
sudo cp -a /etc/ssh/sshd_config /root/backups_interciclo/sshd_config.original
```

La directiva configurada fue:

```conf
Port 2222
```

Se verificó con:

```bash
sudo grep -n '^Port' /etc/ssh/sshd_config
```

**Figura 16.** Verificación de la directiva `Port 2222` en `/etc/ssh/sshd_config`.  
*Referencia: informe PDF original, página 12.*

### 2.2 Validación de sintaxis y estado del demonio sshd

Se comprobó la sintaxis y el estado del servicio:

```bash
sudo sshd -t
sudo systemctl status sshd
```

La documentación fuente registra el servicio SSH activo y escuchando en el puerto personalizado.

**Figura 17.** Prueba de sintaxis y estado activo del servidor OpenSSH en el puerto 2222.  
*Referencia: informe PDF original, página 12.*

### 2.3 Verificación de sockets activos en el puerto 2222

Se inspeccionaron los sockets TCP en escucha:

```bash
sudo ss -tlnp | grep 2222
```

La evidencia confirma que `sshd` escucha en `0.0.0.0:2222` y `[::]:2222`.

**Figura 18.** Confirmación de sockets TCP en escucha sobre el puerto 2222 mediante `ss`.  
*Referencia: informe PDF original, página 13.*

## 3. Seguridad SELinux y firewall

### 3.1 Contexto de seguridad en SELinux

Se verificó el modo de SELinux y la política de puertos:

```bash
getenforce
sudo semanage port -l | grep ssh
```

La evidencia registra SELinux en modo `Enforcing` y el puerto TCP 2222 asociado al tipo `ssh_port_t`.

**Figura 19.** Modo Enforcing de SELinux y asignación del puerto 2222 al contexto `ssh_port_t`.  
*Referencia: informe PDF original, página 13.*

### 3.2 Configuración de reglas en el cortafuegos (firewalld)

Se permitió el puerto SSH personalizado y se recargaron las reglas:

```bash
sudo firewall-cmd --permanent --add-port=2222/tcp
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

El informe fuente muestra la zona activa `public`, con los servicios `dns`, `ftp`, `http`, `ntp` y `ssh`, además del puerto `2222/tcp` y el rango `40000-40100/tcp`.

**Figura 20.** Listado completo de servicios y puertos permitidos en la zona `public` de firewalld.  
*Referencia: informe PDF original, página 14.*

## 4. Pruebas de aceptación

### 4.1 Prueba de acceso remoto SSH desde cliente Windows

Desde Windows PowerShell se ejecutó la conexión utilizando el dominio y el puerto personalizado:

```powershell
ssh -p 2222 estudiantes@ssh.bueno.lab
```

La sesión se estableció correctamente. En la terminal remota se verificó la identidad del servidor y del usuario:

```bash
hostname
whoami
```

Los resultados fueron `srv-bueno` y `estudiantes`, respectivamente.

**Figura 21.** Inicio de sesión remoto exitoso mediante SSH en el puerto 2222 usando el dominio `ssh.bueno.lab`.  
*Referencia: informe PDF original, página 14.*

## 5. Resultado del acceso remoto seguro

La práctica documenta la configuración de OpenSSH en el puerto 2222, la autorización de ese puerto en SELinux y firewalld, y una conexión autenticada desde Windows utilizando el nombre `ssh.bueno.lab`.

> **Nota sobre las imágenes:** las figuras 16–21 están identificadas en el PDF original, entre las páginas 12 y 14. Para visualizarlas incrustadas en GitHub, deben extraerse del PDF y subirse al repositorio como archivos de imagen independientes.

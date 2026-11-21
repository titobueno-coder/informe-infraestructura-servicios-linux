# 03 - Acceso remoto SSH

## 01. Arquitectura y requisitos

Se configuró OpenSSH para aceptar conexiones en el puerto TCP personalizado `2222`. El servidor se identifica como `srv-bueno`, con el usuario `estudiantes` y el nombre de conexión `ssh.bueno.lab`.

| Parámetro | Valor |
|---|---|
| Servicio | `sshd.service` |
| Archivo | `/etc/ssh/sshd_config` |
| Puerto personalizado | `2222/TCP` |
| Usuario remoto | `estudiantes` |
| Nombre de conexión | `ssh.bueno.lab` |
| Hostname | `srv-bueno` |

## 02. Despliegue y configuración

Antes de editar el archivo se guardó una copia:

```bash
sudo cp -a /etc/ssh/sshd_config /root/backups_interciclo/sshd_config.original
```

La directiva activa configurada fue:

```conf
Port 2222
```

Se validó la sintaxis y se reinició el servicio:

```bash
sudo sshd -t
sudo systemctl restart sshd
sudo systemctl status sshd
sudo ss -tlnp | grep 2222
```

La evidencia muestra el demonio escuchando en `0.0.0.0:2222` y `[::]:2222`.

## 03. Seguridad SELinux y firewall

Se autorizó el puerto 2222 para el tipo SELinux de SSH:

```bash
sudo semanage port -a -t ssh_port_t -p tcp 2222
sudo semanage port -l | grep ssh
getenforce
```

La verificación documentada mostró el puerto TCP 2222 asociado a `ssh_port_t` y SELinux en modo `Enforcing`.

El puerto personalizado se permitió en firewalld:

```bash
sudo firewall-cmd --permanent --add-port=2222/tcp
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

La zona activa `public` mostró el puerto `2222/tcp` y el rango `40000-40100/tcp`.

## 04. Pruebas de aceptación y solución de problemas

Desde Windows PowerShell se estableció la conexión mediante el nombre de dominio y el puerto personalizado:

```powershell
ssh -p 2222 estudiantes@ssh.bueno.lab
```

En la primera conexión se aceptó la huella de la clave del servidor y luego se autenticó el usuario. La sesión remota mostró el prompt `estudiantes@srv-bueno`. Dentro de la sesión se comprobó:

```bash
hostname
whoami
```

Los resultados fueron `srv-bueno` y `estudiantes`, respectivamente.

**Evidencias descritas en el PDF:** figuras 16 a 21 (páginas 12 a 14).

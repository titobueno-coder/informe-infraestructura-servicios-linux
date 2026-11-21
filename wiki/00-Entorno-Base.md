# 00 - Entorno base

## 01. Arquitectura y requisitos

La práctica se desarrolló sobre una máquina virtual con CentOS Stream 10. El servidor utiliza la interfaz `enp0s3`, con dirección IPv4 `10.0.2.15/24`. La cuenta utilizada para la administración es `estudiantes` y el nombre asignado al equipo es `srv-bueno`.

| Parámetro | Valor verificado |
|---|---|
| Sistema operativo | CentOS Stream 10 |
| Hostname | `srv-bueno` |
| Usuario de sesión | `estudiantes` |
| Interfaz principal | `enp0s3` |
| IPv4 | `10.0.2.15/24` |
| Carpeta de respaldos | `/root/backups_interciclo/` |

## 02. Despliegue y configuración

Los comandos utilizados para identificar el entorno fueron:

```bash
hostname
whoami
ip addr
ping -c 10 8.8.8.8
```

Antes de modificar archivos de configuración de servicios se almacenaron copias originales en:

```text
/root/backups_interciclo/
```

Los respaldos verificados fueron:

- `named.conf.original`
- `sshd_config.original`

Para comprobarlos:

```bash
sudo ls -lh /root/backups_interciclo/
```

## 03. Seguridad y respaldos

Se mantuvieron copias de los archivos de configuración antes de realizar cambios en BIND y OpenSSH. Esta medida permite conservar una versión original para recuperación en caso de errores.

## 04. Pruebas de aceptación y observaciones

La identidad del servidor quedó confirmada con `hostname` y `whoami`. La interfaz `enp0s3` presentó la dirección `10.0.2.15/24`.

La prueba ICMP documentada hacia `8.8.8.8` transmitió 10 paquetes y recibió 6, con un 40 % de pérdida y una latencia promedio de 39.326 ms. Por tanto, la conectividad externa fue parcial durante esa medición, no una respuesta sin pérdida.

**Evidencias descritas en el PDF:** figuras 1 a 4 (páginas 1 a 3).

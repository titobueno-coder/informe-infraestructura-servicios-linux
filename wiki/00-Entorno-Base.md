# 00 - Entorno base

## 1. Identificación del sistema, conectividad y respaldos

### 1.1 Verificación del nombre de host y usuario activo

Se comprobó la identidad del equipo y de la sesión administrativa mediante los comandos `hostname` y `whoami`. Los resultados identifican al servidor como `srv-bueno` y al usuario de trabajo como `estudiantes`.

```bash
hostname
whoami
```

**Figura 1.** Comprobación del nombre de host (`srv-bueno`) y usuario de sesión (`estudiantes`).  
*Referencia: informe PDF original, página 2.*

### 1.2 Configuración de interfaces de red

Mediante `ip addr` se inspeccionaron las interfaces disponibles. Se verificó que la interfaz principal `enp0s3` estaba operativa y tenía asignada la dirección IPv4 `10.0.2.15/24`.

```bash
ip addr
```

**Figura 2.** Direccionamiento IPv4 e IPv6 asignado a las interfaces `lo` y `enp0s3`.  
*Referencia: informe PDF original, página 2.*

### 1.3 Prueba de conectividad externa (ICMP)

Se realizó una prueba de conectividad hacia `8.8.8.8` mediante diez paquetes ICMP:

```bash
ping -c 10 8.8.8.8
```

El informe fuente registra una latencia promedio de 39.326 ms y describe la prueba como exitosa.

**Figura 3.** Resultados de conectividad externa mediante paquetes ICMP hacia `8.8.8.8`.  
*Referencia: informe PDF original, página 3.*

### 1.4 Verificación de copias de seguridad

Antes de modificar configuraciones de servicios se almacenaron copias originales en `/root/backups_interciclo/`. Se comprobó la existencia de los archivos `named.conf.original` y `sshd_config.original`.

```bash
sudo ls -lh /root/backups_interciclo/
```

**Figura 4.** Listado de archivos de respaldo originales de DNS y SSH.  
*Referencia: informe PDF original, página 3.*

## 2. Resultado del entorno base

La identificación del servidor, la interfaz de red y los respaldos quedaron documentados como base para la instalación y configuración de DNS, HTTPD y SSH.

> **Nota sobre las imágenes:** las figuras 1–4 están identificadas en el PDF original. Para mostrarlas incrustadas en esta página de GitHub, deben extraerse del PDF y subirse al repositorio como archivos de imagen independientes.

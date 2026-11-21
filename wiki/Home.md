# Informe técnico: Infraestructura de Servicios de Red en Linux

**Estudiante:** Adrian Bueno  
**Sistema operativo:** CentOS Stream 10  
**Hostname:** `srv-bueno`  
**Usuario:** `estudiantes`  
**Fecha registrada en las evidencias:** 21 de noviembre de 2026

## Presentación

Este informe documenta la implementación, configuración y verificación de servicios esenciales de red en un servidor CentOS Stream 10. La práctica comprende la identificación del sistema, conectividad, respaldos de configuración, resolución de nombres con BIND (DNS), publicación de un sitio web con Apache HTTP Server y acceso remoto seguro mediante OpenSSH. También se documentan los controles de SELinux y firewalld.

## Datos técnicos del entorno

| Elemento | Valor |
|---|---|
| Hostname | `srv-bueno` |
| Usuario | `estudiantes` |
| Interfaz de red | `enp0s3` |
| Dirección IPv4 | `10.0.2.15/24` |
| Dominio DNS | `bueno.lab` |
| Servidor DNS | `named` |
| Servidor web | Apache HTTP Server 2.4.63 |
| Raíz web | `/var/www/html` |
| Puerto HTTP | `80/TCP` |
| Puerto SSH personalizado | `2222/TCP` |
| SELinux | `Enforcing` |

## Navegación

- [00 - Entorno base](00-Entorno-Base.md)
- [01 - Servicio DNS (BIND)](01-DNS.md)
- [02 - Servicio HTTPD (Apache)](02-HTTPD.md)
- [03 - Acceso remoto SSH](03-SSH.md)

## Resultado general

Las evidencias documentan la resolución local de los nombres DNS, el estado activo de los servicios `named` y `httpd`, la respuesta HTTP `200 OK`, y una conexión SSH exitosa desde Windows hacia `ssh.bueno.lab` utilizando el puerto TCP 2222.

> **Nota sobre evidencias:** Las capturas descritas corresponden al informe PDF entregado como fuente. Este repositorio contiene la documentación en Markdown; el PDF y sus imágenes no se han adjuntado como archivos independientes.

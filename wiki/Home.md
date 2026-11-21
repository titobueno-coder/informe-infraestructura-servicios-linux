# Informe técnico: Infraestructura de Servicios de Red en Linux

**Estudiante:** Adrian Bueno  
**Sistema operativo:** CentOS Stream 10  
**Nombre del servidor:** `srv-bueno`  
**Usuario:** `estudiantes`  
**Fecha registrada en las evidencias:** 21 de noviembre de 2026

---

## 1. Presentación

El presente informe documenta la implementación, configuración y verificación de servicios esenciales de red en un servidor CentOS Stream 10. La práctica comprende la identificación del sistema, conectividad, respaldos de configuración, resolución de nombres mediante BIND (DNS), publicación de un sitio web con Apache HTTP Server y acceso remoto seguro mediante OpenSSH. También se describen los controles de seguridad SELinux y firewalld.

## 2. Información técnica del entorno

| Elemento | Valor verificado |
|---|---|
| Hostname | `srv-bueno` |
| Usuario | `estudiantes` |
| Interfaz de red | `enp0s3` |
| Dirección IPv4 | `10.0.2.15/24` |
| Dominio DNS | `bueno.lab` |
| Servicio DNS | `named` |
| Servidor web | Apache HTTP Server 2.4.63 |
| Directorio web | `/var/www/html` |
| Puerto HTTP | `80/TCP` |
| Puerto SSH personalizado | `2222/TCP` |
| SELinux | `Enforcing` |

## 3. Organización del informe

La documentación se divide en cuatro capítulos técnicos. Cada capítulo presenta los requisitos, los pasos de configuración, las medidas de seguridad y las pruebas de aceptación.

1. [00 - Entorno base](00-Entorno-Base.md)
2. [01 - Configuración y validación DNS (BIND)](01-DNS.md)
3. [02 - Implementación del servidor web HTTPD (Apache)](02-HTTPD.md)
4. [03 - Configuración del acceso remoto seguro SSH](03-SSH.md)

## 4. Resultados generales

Las evidencias del informe fuente documentan:

- Identificación del servidor como `srv-bueno` y del usuario `estudiantes`.
- Configuración de la zona DNS `bueno.lab` y resolución local de los nombres registrados.
- Servicio Apache activo y respuestas HTTP con estado `200 OK`.
- Acceso SSH remoto mediante `ssh.bueno.lab`, utilizando el puerto TCP `2222`.
- SELinux en modo `Enforcing` y reglas de firewalld para los servicios configurados.

## 5. Evidencias y referencias

Las figuras se identifican y describen en el informe PDF fuente, siguiendo su numeración original:

| Capítulo | Figuras del PDF | Páginas del PDF |
|---|---:|---:|
| Entorno base | 1–4 | 1–3 |
| DNS (BIND) | 5–9 | 3–7 |
| HTTPD (Apache) | 10–15 | 7–11 |
| SSH | 16–21 | 12–14 |

> **Nota:** esta carpeta contiene la documentación Markdown. Las imágenes originales están dentro del PDF fuente y todavía no se han incorporado como archivos de imagen independientes al repositorio.

---
**Autor:** Adrian Bueno  
**Práctica individual — Infraestructura de Servicios de Red en Linux**

# 02 - Despliegue y verificación del servidor web (Apache HTTPD)

## 1. Arquitectura y requisitos

Se utilizó Apache HTTP Server en CentOS Stream 10 para publicar una página web personalizada.

| Parámetro | Valor |
|---|---|
| Servicio | `httpd.service` |
| Versión documentada | Apache 2.4.63 |
| Puerto | `80/TCP` |
| Directorio raíz | `/var/www/html` |
| Página principal | `/var/www/html/index.html` |
| Logotipo institucional | `/var/www/html/imagenes/logo.png` |
| Dominio utilizado en la prueba | `web.bueno.lab` |

## 2. Despliegue y configuración

### 2.1 Estado operativo del servicio httpd

Se habilitó y arrancó Apache y se comprobó su estado:

```bash
sudo systemctl enable --now httpd
sudo systemctl status httpd
```

El informe fuente registra el servicio `httpd.service` habilitado y en ejecución activa.

**Figura 10.** Estado de ejecución del servidor Apache HTTP (`httpd.service`).  

![Figura 10 - evidencia original](../evidencias/figura-10.png)
*Referencia: informe PDF original, página 8.*

### 2.2 Verificación de cabeceras HTTP locales

Se inspeccionaron los encabezados de respuesta del servidor web:

```bash
curl -I http://localhost
```

La respuesta registrada fue `HTTP/1.1 200 OK`, emitida por Apache/2.4.63 (CentOS Stream).

![Figura 11 - evidencia original](../evidencias/figura-11.png)

**Figura 11.** Respuesta HTTP 200 OK obtenida mediante `curl` hacia localhost.  
*Referencia: informe PDF original, página 8.*

### 2.3 Verificación de resolución web para web.bueno.lab

Se comprobó la respuesta HTTP para el nombre web mediante resolución forzada hacia la dirección local:


![Figura 12 - evidencia original](../evidencias/figura-12.png)
```bash
curl -I --resolve web.bueno.lab:80:127.0.0.1 http://web.bueno.lab
```

La respuesta registrada fue `HTTP/1.1 200 OK`.

**Figura 12.** Validación de cabeceras HTTP para el dominio `web.bueno.lab`.  
*Referencia: informe PDF original, página 9.*

### 2.4 Estructura del documento web (index.html)

Se inspeccionó el archivo principal alojado en `/var/www/html/index.html`. El documento contiene una página HTML5 personalizada titulada “CentOS Stream 10 | Adrian Bueno”, con estilos CSS y una clase para mostrar el logotipo.

![Figura 13 - evidencia original](../evidencias/figura-13.png)

```bash
sudo cat /var/www/html/index.html
```

La referencia del logotipo en el HTML es:

```html
<img src="imagenes/logo.png" alt="Logo UCuenca Tec" class="logo">
```

**Figura 13.** Inspección del archivo principal `index.html` en el directorio raíz web.  
*Referencia: informe PDF original, página 10.*

### 2.5 Verificación de recursos estáticos (imágenes)

Se comprobó la existencia y disponibilidad del archivo `logo.png` dentro del directorio de imágenes:

![Figura 14 - evidencia original](../evidencias/figura-14.png)

```bash
ls -lh /var/www/html/imagenes/
curl -I http://localhost/imagenes/logo.png
```

El informe fuente registra permisos de lectura para el archivo y una respuesta HTTP `200 OK` con tipo MIME `image/png`.

**Figura 14.** Verificación de permisos y disponibilidad HTTP del recurso `logo.png`.  

![Figura 15 - evidencia original](../evidencias/figura-15.png)
*Referencia: informe PDF original, página 11.*

### 2.6 Visualización del sitio web en navegador

Se abrió `http://localhost` en un navegador gráfico. La evidencia muestra el logotipo institucional UCUENCATEC, la identificación del autor como ADRIAN BUENO y el encabezado de bienvenida a CentOS Stream 10.

**Figura 15.** Renderizado final del portal web institucional en el navegador.  
*Referencia: informe PDF original, página 11.*

## 3. Seguridad SELinux y firewall

Se habilitó el servicio HTTP en firewalld y se recargaron las reglas:

```bash
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --reload
sudo firewall-cmd --list-services
```

También se restauraron los contextos SELinux del contenido web:

```bash
sudo restorecon -Rv /var/www/html/
getenforce
```

El estado de SELinux registrado en la práctica fue `Enforcing`.

## 4. Pruebas de aceptación y resultado

Las pruebas documentadas confirmaron respuestas HTTP satisfactorias tanto para `localhost` como para `web.bueno.lab`. También se verificó la disponibilidad del logotipo institucional y la visualización de la página personalizada.

> **Nota sobre las imágenes:** las figuras 10–15 están identificadas en el PDF original, entre las páginas 8 y 11. Para mostrarlas incrustadas en GitHub, deben extraerse del PDF y subirse al repositorio como archivos de imagen independientes.

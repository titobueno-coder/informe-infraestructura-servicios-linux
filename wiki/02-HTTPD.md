# 02 - Servicio HTTPD (Apache)

## 01. Arquitectura y requisitos

Se utilizó Apache HTTP Server en CentOS Stream 10 para publicar una página institucional personalizada.

| Parámetro | Valor |
|---|---|
| Servicio | `httpd.service` |
| Versión documentada | Apache 2.4.63 |
| Puerto | `80/TCP` |
| Directorio raíz | `/var/www/html` |
| Página principal | `/var/www/html/index.html` |
| Recurso gráfico | `/var/www/html/imagenes/logo.png` |
| Dominio utilizado en la prueba | `web.bueno.lab` |

## 02. Despliegue y configuración

Se habilitó y arrancó Apache:

```bash
sudo systemctl enable --now httpd
sudo systemctl status httpd
```

La página principal `index.html` contiene una presentación de CentOS Stream 10, el nombre del estudiante Adrian Bueno y una descripción de los servicios trabajados. El logotipo institucional se almacena en `imagenes/logo.png` y se referencia desde el HTML mediante:

```html
<img src="imagenes/logo.png" alt="Logo UCuenca Tec" class="logo">
```

La estructura de recursos documentada es:

```text
/var/www/html/
├── index.html
└── imagenes/
    └── logo.png
```

## 03. Seguridad SELinux y firewall

El servicio HTTP fue habilitado en firewalld y se recargaron las reglas:

```bash
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --reload
sudo firewall-cmd --list-services
```

Se restauraron los contextos SELinux del contenido web:

```bash
sudo restorecon -Rv /var/www/html/
```

La práctica mantuvo SELinux en modo `Enforcing`.

## 04. Pruebas de aceptación y solución de problemas

Se comprobó la respuesta HTTP local:

```bash
curl -I http://localhost
```

La respuesta registrada fue `HTTP/1.1 200 OK`.

También se probó el nombre web:

```bash
curl -I --resolve web.bueno.lab:80:127.0.0.1 http://web.bueno.lab
```

La respuesta fue igualmente `HTTP/1.1 200 OK`.

Se verificó el recurso gráfico y sus permisos:

```bash
ls -lh /var/www/html/imagenes/
curl -I http://localhost/imagenes/logo.png
```

El archivo `logo.png` fue servido con tipo MIME `image/png` y estado `200 OK`. Finalmente, se abrió `http://localhost` en el navegador para comprobar el logotipo, el nombre ADRIAN BUENO y el encabezado de bienvenida a CentOS Stream 10.

**Evidencias descritas en el PDF:** figuras 10 a 15 (páginas 7 a 11).

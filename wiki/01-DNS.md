# 01 - Configuración y validación del servidor DNS (BIND)

## 1. Arquitectura y requisitos

Se configuró BIND como servidor DNS autoritativo para la zona directa `bueno.lab`. El servidor utiliza la dirección IPv4 `10.0.2.15`.

| Parámetro | Valor |
|---|---|
| Servicio | `named.service` |
| Zona | `bueno.lab` |
| Configuración principal | `/etc/named.conf` |
| Archivo de zona | `/var/named/bueno.lab.zone` |
| Puerto DNS | `53` |
| Serial | `2026112001` |

## 2. Despliegue y configuración

### 2.1 Parámetros globales de configuración (named.conf)

Se instalaron los paquetes BIND y sus utilidades:

```bash
sudo dnf install -y bind bind-utils
```

En `/etc/named.conf` se configuraron las directivas de escucha y consulta:

```conf
listen-on port 53 { any; };
listen-on-v6 port 53 { any; };
allow-query { any; };
```

La declaración de zona utilizada fue:

```conf
zone "bueno.lab" IN {
    type master;
    file "bueno.lab.zone";
    allow-update { none; };
};
```

**Figura 5.** Directivas globales del servicio BIND en `/etc/named.conf`.  
*Referencia: informe PDF original, página 3.*

### 2.2 Archivo de zona directa (bueno.lab.zone)

El archivo `/var/named/bueno.lab.zone` establece un TTL de 86400 segundos, el registro SOA, el registro NS y los registros A para el dominio raíz, `ns1`, `web` y `ssh`, todos apuntando a `10.0.2.15`.

```dns
$TTL 86400
@ IN SOA ns1.bueno.lab. admin.bueno.lab. (
    2026112001
    3600
    1800
    604800
    86400
)

@       IN NS  ns1.bueno.lab.
@       IN A   10.0.2.15
ns1     IN A   10.0.2.15
web     IN A   10.0.2.15
ssh     IN A   10.0.2.15
```

**Figura 6.** Registros SOA, NS y A configurados para la zona `bueno.lab`.  
*Referencia: informe PDF original, página 4.*

### 2.3 Validación de sintaxis de configuración y zona

Se validaron la configuración general y el archivo de zona con:

```bash
sudo named-checkconf
sudo named-checkzone bueno.lab /var/named/bueno.lab.zone
```

La validación de zona devolvió `OK` con el serial `2026112001`.

**Figura 7.** Verificación sintáctica de archivos de configuración y zona DNS.  
*Referencia: informe PDF original, página 5.*

### 2.4 Estado operativo del servicio named

Se habilitó el servicio en el arranque y se comprobó su estado:

```bash
sudo systemctl enable --now named
sudo systemctl status named
```

El informe fuente registra el servicio `named.service` habilitado y en estado `active (running)`.

**Figura 8.** Estado activo y registros de inicio del servicio `named`.  
*Referencia: informe PDF original, página 6.*

## 3. Seguridad SELinux y firewall

Se ajustaron el propietario, los permisos y el contexto SELinux del archivo de zona:

```bash
sudo chown root:named /var/named/bueno.lab.zone
sudo chmod 640 /var/named/bueno.lab.zone
sudo restorecon -v /var/named/bueno.lab.zone
```

Se permitió DNS en firewalld y se recargaron las reglas:

```bash
sudo firewall-cmd --permanent --add-service=dns
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

## 4. Pruebas de aceptación y resolución de problemas

### 4.1 Pruebas de resolución de nombres con dig

Se realizaron consultas directas contra el servidor DNS local:

```bash
dig @127.0.0.1 bueno.lab
dig @127.0.0.1 web.bueno.lab
dig @127.0.0.1 ssh.bueno.lab
```

Las respuestas registradas devolvieron estado `NOERROR` y resolvieron los nombres consultados hacia `10.0.2.15`.

**Figura 9.** Resolución exitosa de registros tipo A mediante la utilidad `dig`.  
*Referencia: informe PDF original, página 7.*

## 5. Resultado del servicio DNS

La zona `bueno.lab` quedó validada y el servicio BIND se documentó como activo, con consultas locales exitosas para el dominio raíz y los nombres `web` y `ssh`.

> **Nota sobre las imágenes:** la figura 5 corresponde a la página 3 del PDF; la figura 6 a la página 4; las figuras 7 y 8 a las páginas 5 y 6; y la figura 9 a la página 7. Para visualizarlas dentro de GitHub, deben extraerse del PDF y subirse como imágenes independientes.

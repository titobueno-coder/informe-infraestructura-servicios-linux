# 01 - Servicio DNS (BIND)

## 01. Arquitectura y requisitos

Se configuró BIND como servidor DNS autoritativo para la zona directa `bueno.lab`. El servidor utiliza la dirección `10.0.2.15`.

| Parámetro | Valor |
|---|---|
| Servicio | `named.service` |
| Zona | `bueno.lab` |
| Archivo de configuración | `/etc/named.conf` |
| Archivo de zona | `/var/named/bueno.lab.zone` |
| Puerto | `53` |
| Serial | `2026112001` |

## 02. Despliegue y configuración

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

El archivo `/var/named/bueno.lab.zone` define los siguientes registros:

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

Se ajustaron propietario, permisos y contexto SELinux del archivo de zona:

```bash
sudo chown root:named /var/named/bueno.lab.zone
sudo chmod 640 /var/named/bueno.lab.zone
sudo restorecon -v /var/named/bueno.lab.zone
```

## 03. Seguridad SELinux y firewall

El servicio DNS se habilitó en firewalld:

```bash
sudo firewall-cmd --permanent --add-service=dns
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

El archivo de zona recibió el contexto SELinux correspondiente mediante `restorecon`. La evidencia de configuración muestra el servicio DNS habilitado en el cortafuegos.

## 04. Pruebas de aceptación y solución de problemas

Se validó la sintaxis general y el archivo de zona:

```bash
sudo named-checkconf
sudo named-checkzone bueno.lab /var/named/bueno.lab.zone
```

La comprobación de zona devolvió `OK`. Después se habilitó y arrancó el servicio:

```bash
sudo systemctl enable --now named
sudo systemctl status named
```

Las consultas locales documentadas fueron:

```bash
dig @127.0.0.1 bueno.lab
dig @127.0.0.1 web.bueno.lab
dig @127.0.0.1 ssh.bueno.lab
```

Las respuestas mostraron estado `NOERROR` y dirección `10.0.2.15`.

**Evidencias descritas en el PDF:** figuras 5 a 9 (páginas 3 a 7).

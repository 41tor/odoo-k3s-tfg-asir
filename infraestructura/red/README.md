# Red

Configuracion de red usada por las maquinas virtuales del laboratorio.

Cada nodo tiene dos interfaces:

| Interfaz | Uso |
| --- | --- |
| `ens160` | Red NAT de VMware, usada para salida a Internet y acceso desde el host |
| `ens256` | Red interna del cluster, usada para la comunicacion entre nodos |

## Direccionamiento

| Nodo | IP NAT | IP interna |
| --- | --- | --- |
| `k3s-master` | `172.16.214.141` | `192.168.10.10` |
| `k3s-worker1` | `172.16.214.142` | `192.168.10.20` |
| `k3s-worker2` | `172.16.214.143` | `192.168.10.30` |

Los archivos Netplan de cada nodo estan en esta misma carpeta.

## Dominios locales

Los servicios publicados mediante Traefik se acceden desde el host usando dominios locales:

| Servicio | Dominio |
| --- | --- |
| Odoo | `garciaa.odoo.local` |
| Grafana | `garciaa.grafana.local` |


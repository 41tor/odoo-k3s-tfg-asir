# Infraestructura

Esta carpeta recoge la parte base del laboratorio: maquinas virtuales, red y diagramas generales.

El entorno se ha montado sobre VMware Fusion con tres maquinas Ubuntu Server:

| Maquina | Rol | CPU | RAM | Sistema |
| --- | --- | ---: | ---: | --- |
| `k3s-master` | Control plane | 2 | 4 GB | Ubuntu Server 24.04.4 LTS |
| `k3s-worker1` | Worker | 2 | 4 GB | Ubuntu Server 24.04.4 LTS |
| `k3s-worker2` | Worker | 2 | 4 GB | Ubuntu Server 24.04.4 LTS |

## Carpetas

| Carpeta | Contenido |
| --- | --- |
| `red/` | Configuracion Netplan e IPs de los nodos |
| `vms/` | Resumen de maquinas virtuales y roles |
| `diagramas/` | Diagramas de arquitectura, red, almacenamiento y acceso |


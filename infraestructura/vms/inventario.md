# Inventario de maquinas virtuales

Resumen de las maquinas usadas en el laboratorio.

| Maquina | Rol | CPU | RAM | Sistema | Arquitectura |
| --- | --- | ---: | ---: | --- | --- |
| `k3s-master` | Control plane | 2 | 4 GB | Ubuntu Server 24.04.4 LTS | arm64 |
| `k3s-worker1` | Worker | 2 | 4 GB | Ubuntu Server 24.04.4 LTS | arm64 |
| `k3s-worker2` | Worker | 2 | 4 GB | Ubuntu Server 24.04.4 LTS | arm64 |

## Red

| Maquina | IP NAT | IP interna |
| --- | --- | --- |
| `k3s-master` | `172.16.214.141` | `192.168.10.10` |
| `k3s-worker1` | `172.16.214.142` | `192.168.10.20` |
| `k3s-worker2` | `172.16.214.143` | `192.168.10.30` |

## Criterios

- El nodo maestro queda reservado para la gestion del cluster.
- Los workers ejecutan las cargas principales.
- La red interna se usa para la comunicacion entre nodos.
- La red NAT se usa para salida a Internet y acceso desde el equipo anfitrion.

No se versionan imagenes completas de las maquinas virtuales, discos `.vmdk` ni exports `.ova`.

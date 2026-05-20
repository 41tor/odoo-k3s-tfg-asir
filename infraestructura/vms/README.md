# Maquinas virtuales

El laboratorio se ejecuta sobre tres maquinas virtuales Ubuntu Server en VMware Fusion.

| Maquina | Rol | Funcion |
| --- | --- | --- |
| `k3s-master` | Control plane | Gestion del cluster K3s |
| `k3s-worker1` | Worker | Ejecucion de cargas y almacenamiento Longhorn |
| `k3s-worker2` | Worker | Ejecucion de cargas y almacenamiento Longhorn |

## Recursos

| Maquina | CPU | RAM | Arquitectura |
| --- | ---: | ---: | --- |
| `k3s-master` | 2 | 4 GB | arm64 |
| `k3s-worker1` | 2 | 4 GB | arm64 |
| `k3s-worker2` | 2 | 4 GB | arm64 |

El nodo maestro se reserva para tareas de control y los servicios principales se ejecutan en los workers.

## Archivos

| Archivo | Contenido |
| --- | --- |
| `inventario.md` | Resumen de maquinas, recursos y direcciones |

No se suben discos de maquinas virtuales ni exports de VMware. Esos archivos son demasiado grandes y pueden contener datos internos del sistema.

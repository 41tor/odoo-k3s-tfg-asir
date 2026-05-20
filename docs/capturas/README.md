# Capturas

Capturas realizadas durante el desarrollo del proyecto. Estan organizadas por partes de la infraestructura y por pruebas.

## Implementacion

| Carpeta | Contenido |
| --- | --- |
| `01-maquinas-virtuales/` | Creacion y preparacion de las maquinas del laboratorio |
| `02-red/` | Configuracion de red y conectividad entre nodos |
| `03-k3s/` | Instalacion del master, union de workers y restriccion del nodo maestro |
| `04-longhorn/` | Instalacion y comprobaciones iniciales de Longhorn |
| `05-postgresql/` | Despliegue y verificacion inicial de PostgreSQL |
| `06-odoo/` | Despliegue de Odoo dentro del cluster |
| `07-traefik-https/` | Publicacion mediante Traefik y acceso HTTPS local |
| `08-monitorizacion/` | Instalacion de Prometheus, Grafana y configuracion de monitorizacion |
| `10-repositorio-github/` | Organizacion inicial del repositorio |

## Pruebas

| Carpeta | Contenido |
| --- | --- |
| `09-pruebas/01-odoo/` | Comprobacion del funcionamiento de Odoo |
| `09-pruebas/02-persistencia/` | Persistencia tras recrear el pod |
| `09-pruebas/03-caida-nodo/` | Prueba de caida de un nodo worker |
| `09-pruebas/04-longhorn/` | Estado del almacenamiento Longhorn |
| `09-pruebas/05-monitorizacion/` | Dashboards, alertas y supervision |

Las capturas se mantienen como evidencia completa del proceso seguido.

# Arquitectura

El proyecto se ha montado como un laboratorio local con tres maquinas virtuales Ubuntu Server sobre VMware Fusion. La infraestructura busca separar la aplicacion, la base de datos, el almacenamiento, el acceso externo y la monitorizacion.

## Nodos

| Nodo | Rol | IP interna | Uso |
| --- | --- | --- | --- |
| `k3s-master` | Control plane | `192.168.10.10` | Administracion del cluster |
| `k3s-worker1` | Worker | `192.168.10.20` | Ejecucion de servicios |
| `k3s-worker2` | Worker | `192.168.10.30` | Ejecucion de servicios |

El nodo maestro queda reservado para el control del cluster. Las cargas principales se ejecutan en los workers.

## Red

Cada maquina tiene dos interfaces:

- Una interfaz con NAT para salida a Internet.
- Una interfaz interna con IP fija para la comunicacion entre nodos.

La red interna evita depender de direcciones dinamicas y facilita la union de los workers al cluster K3s.

## Componentes

| Componente | Funcion |
| --- | --- |
| K3s | Orquestacion de contenedores |
| Odoo | Aplicacion ERP |
| PostgreSQL | Base de datos de Odoo |
| Longhorn | Volumenes persistentes |
| Traefik | Entrada HTTP/HTTPS al cluster |
| Prometheus | Recogida de metricas |
| Grafana | Dashboards y alertas |

## Flujo general

```text
Usuario
  |
  | HTTPS local
  v
Traefik Ingress
  |
  +--> Odoo
          |
          v
      PostgreSQL

Longhorn mantiene los volumenes persistentes usados por Odoo,
PostgreSQL y Grafana.
```

## Dominios locales

| Servicio | Dominio |
| --- | --- |
| Odoo | `garciaa.odoo.local` |
| Grafana | `garciaa.grafana.local` |

Estos dominios se resuelven desde el equipo anfitrion mediante el fichero `hosts`.


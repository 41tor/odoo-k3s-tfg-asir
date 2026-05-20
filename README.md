# Despliegue de Odoo sobre K3s

Repositorio del proyecto de despliegue de Odoo sobre K3s.

**Autor:** Aitor Garcia Abellan

## Descripcion

Este proyecto consiste en montar una infraestructura ERP basada en **Odoo 18** sobre un cluster **K3s**. El entorno se ha desplegado en local mediante tres maquinas virtuales Ubuntu Server sobre VMware Fusion: un nodo maestro y dos nodos worker.

La idea principal del trabajo es probar una arquitectura mas cercana a un entorno real que una instalacion tradicional en un unico servidor. Para ello se separan los servicios principales, se usa almacenamiento persistente, se publica la aplicacion mediante HTTPS local y se anade monitorizacion del cluster.

El proyecto no se centra en configurar Odoo a nivel funcional o empresarial, sino en la parte de sistemas: despliegue, red, almacenamiento, persistencia, recuperacion ante fallos y observabilidad.

## Tecnologias utilizadas

- **VMware Fusion** para la virtualizacion del laboratorio.
- **Ubuntu Server 24.04 LTS** como sistema operativo de los nodos.
- **K3s** como distribucion ligera de Kubernetes.
- **Odoo 18** como aplicacion ERP.
- **PostgreSQL 16** como base de datos.
- **Longhorn** para almacenamiento persistente distribuido.
- **Traefik** como Ingress Controller.
- **Prometheus y Grafana** para monitorizacion y alertas.
- **GitHub** para organizar configuraciones, manifiestos y documentacion.

## Objetivos del proyecto

El objetivo general es desplegar una infraestructura ERP con Odoo sobre K3s que tenga persistencia de datos, acceso web seguro, monitorizacion y cierta capacidad de recuperacion ante fallos de servicio o de nodo.

Objetivos concretos:

- Crear un laboratorio con tres maquinas virtuales Ubuntu Server.
- Configurar una red interna con direccionamiento estatico.
- Instalar un cluster K3s con un nodo maestro y dos workers.
- Reservar el nodo maestro para tareas de control.
- Desplegar PostgreSQL como base de datos de Odoo.
- Desplegar Odoo conectado a PostgreSQL dentro del cluster.
- Usar Longhorn para los volumenes persistentes.
- Publicar Odoo y Grafana mediante Traefik.
- Configurar acceso HTTPS con certificados locales.
- Anadir monitorizacion con Prometheus y Grafana.
- Probar persistencia, recreacion de pods y caida de un nodo worker.

## Arquitectura

El laboratorio esta formado por tres nodos:

| Nodo | Rol | Funcion principal |
| --- | --- | --- |
| `k3s-master` | Control plane | Administracion del cluster |
| `k3s-worker1` | Worker | Ejecucion de servicios |
| `k3s-worker2` | Worker | Ejecucion de servicios |

Los servicios se organizan por namespaces. Odoo y PostgreSQL se despliegan separados para mantener una estructura mas clara y parecida a una arquitectura real. Longhorn se encarga de proporcionar volumenes persistentes para conservar datos aunque los pods se reinicien o se vuelvan a crear.

## Servicios principales

| Servicio | Namespace | Uso |
| --- | --- | --- |
| Odoo | `odoo` | Aplicacion ERP |
| PostgreSQL | `postgresql` | Base de datos |
| Longhorn | `longhorn-system` | Almacenamiento persistente |
| Prometheus | `monitoring` | Recogida de metricas |
| Grafana | `monitoring` | Dashboards y alertas |
| Traefik | `kube-system` | Publicacion mediante Ingress |

## Acceso local

El acceso a los servicios se realiza mediante dominios locales configurados en el equipo anfitrion:

| Servicio | Dominio local | Puerto interno |
| --- | --- | --- |
| Odoo | `garciaa.odoo.local` | `8069` |
| Grafana | `garciaa.grafana.local` | `80` |

Para HTTPS se utilizan certificados autofirmados, suficientes para validar el funcionamiento dentro del laboratorio.

## Estructura del repositorio

```text
.
├── docs/
│   ├── capturas/
│   └── memoria/
├── infraestructura/
│   ├── diagramas/
│   ├── red/
│   └── vms/
├── kubernetes/
│   ├── k3s/
│   ├── longhorn/
│   ├── monitorizacion/
│   ├── odoo/
│   ├── postgresql/
│   └── traefik/
├── .gitignore
├── LICENSE
└── README.md
```

## Documentacion rapida

- [Arquitectura](docs/arquitectura.md)
- [Despliegue](docs/despliegue.md)
- [Pruebas](docs/pruebas.md)
- [Incidencias](docs/incidencias.md)
- [Capturas](docs/capturas/)
- [Diagramas de infraestructura](infraestructura/diagramas/)

## Contenido

### `infraestructura/`

Incluye archivos relacionados con la base del entorno: maquinas virtuales, red, recursos asignados y diagramas.

### `kubernetes/`

Contiene los manifiestos organizados por servicio:

- `odoo/`: despliegue de Odoo, servicio, volumen persistente, secreto e Ingress.
- `postgresql/`: despliegue de PostgreSQL con PVC y configuracion de base de datos.
- `monitorizacion/`: recursos relacionados con Grafana y su publicacion.
- `longhorn/`: documentacion y recursos del almacenamiento.
- `traefik/`: configuracion relacionada con el acceso externo.
- `k3s/`: notas y archivos relacionados con el cluster.

### `docs/`

Carpeta destinada a documentacion, capturas y material de apoyo del proyecto.

## Pruebas realizadas

Durante el desarrollo se han comprobado varios puntos importantes:

- Acceso a Odoo desde navegador mediante dominio local.
- Conexion entre Odoo y PostgreSQL dentro del cluster.
- Creacion de datos de prueba en Odoo.
- Persistencia tras eliminar y recrear el pod de Odoo.
- Estado de los PVC gestionados por Longhorn.
- Comportamiento del sistema al apagar un nodo worker.
- Visualizacion del estado del cluster desde Grafana.
- Pruebas de alertas basicas.

## Incidencias encontradas

Durante la implementacion aparecieron varios problemas que se fueron corrigiendo:

- Error de tipo **Multi-Attach** al mover un volumen `ReadWriteOnce` entre nodos.
- Problema de inicializacion de PostgreSQL al usar un volumen Longhorn con contenido previo.
- Perdida inicial de dashboards y alertas de Grafana al no tener persistencia activada.
- Estados inconsistentes al suspender y reanudar maquinas virtuales.

Estas incidencias forman parte del aprendizaje del proyecto, ya que obligaron a revisar logs, estados de pods, volumenes, eventos de Kubernetes y configuracion de los servicios.

## Memoria del proyecto

La memoria completa del proyecto se mantiene como documento aparte. Este repositorio contiene la parte tecnica y los archivos necesarios para entender como se ha organizado y desplegado la infraestructura.

## Seguridad

Las credenciales reales, tokens, kubeconfig y copias de seguridad con datos sensibles no deben subirse al repositorio. Los secretos incluidos en los manifiestos estan pensados como valores de ejemplo o placeholders.

## Licencia

El contenido tecnico de este repositorio se publica bajo licencia MIT.

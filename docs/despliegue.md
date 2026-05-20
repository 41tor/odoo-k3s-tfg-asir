# Despliegue

Resumen de los pasos seguidos para montar la infraestructura.

## 1. Preparacion de maquinas

Se crean tres maquinas virtuales Ubuntu Server 24.04 LTS:

- `k3s-master`
- `k3s-worker1`
- `k3s-worker2`

Despues se ajustan hostname, red, SSH y direccionamiento estatico.

Las configuraciones Netplan estan en:

- `infraestructura/red/k3s-master-netplan.yaml`
- `infraestructura/red/k3s-worker1-netplan.yaml`
- `infraestructura/red/k3s-worker2-netplan.yaml`

## 2. Instalacion del cluster K3s

Primero se instala K3s en el nodo maestro. Despues se unen los dos workers usando el token generado por el master.

La comprobacion principal es que los tres nodos aparezcan en estado `Ready`.

Tambien se aplica una restriccion al nodo maestro para que las cargas normales se ejecuten en los workers.

## 3. Almacenamiento con Longhorn

Longhorn se utiliza como sistema de almacenamiento persistente. Los PVC de Odoo, PostgreSQL y Grafana usan la clase de almacenamiento `longhorn`.

El objetivo es que los datos no dependan del ciclo de vida de los pods.

## 4. PostgreSQL

PostgreSQL se despliega en el namespace `postgresql`.

Manifiesto principal:

- `kubernetes/postgresql/postgresql.yaml`

Recursos definidos:

- Namespace
- Secret
- PersistentVolumeClaim
- Deployment
- Service interno

En el manifiesto se define `PGDATA` en una subcarpeta del volumen para evitar problemas de inicializacion con `lost+found`.

## 5. Odoo

Odoo se despliega en el namespace `odoo`.

Manifiestos principales:

- `kubernetes/odoo/odoo.yaml`
- `kubernetes/odoo/odoo-ingress.yaml`

Odoo se conecta a PostgreSQL mediante DNS interno:

```text
postgresql.postgresql.svc.cluster.local
```

El despliegue incluye un PVC para `/var/lib/odoo` y un `initContainer` para preparar permisos antes de arrancar la aplicacion.

## 6. Acceso externo

Traefik publica Odoo y Grafana mediante recursos Ingress.

Dominios usados:

- `garciaa.odoo.local`
- `garciaa.grafana.local`

El acceso HTTPS se valida con certificados autofirmados.

## 7. Monitorizacion

La monitorizacion se basa en Prometheus y Grafana, desplegados en el namespace `monitoring`.

Grafana se publica mediante:

- `kubernetes/monitorizacion/grafana-ingress.yaml`

Tambien se habilita persistencia para evitar perder dashboards y alertas al recrear el pod.

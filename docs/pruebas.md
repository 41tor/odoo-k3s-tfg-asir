# Pruebas

Las pruebas se hicieron para comprobar que la infraestructura funcionaba y que los datos no se perdian ante recreaciones de pods o caidas de nodos.

## Resumen

| Prueba | Que se comprueba | Resultado |
| --- | --- | --- |
| Acceso a Odoo | La aplicacion responde por navegador | Correcto |
| Alta de contacto | Odoo escribe datos en PostgreSQL | Correcto |
| Recreacion de pod | Odoo vuelve a arrancar tras eliminar el pod | Correcto |
| Persistencia | El dato creado sigue disponible | Correcto |
| Caida de worker | Kubernetes reprograma la carga | Correcto, con tiempo de recuperacion |
| Longhorn | Los volumenes siguen accesibles | Correcto, con estado `degraded` temporal |
| Grafana | Se ven metricas del cluster | Correcto |
| Alertas | Grafana permite definir y probar avisos | Correcto |

## Acceso a Odoo

Se comprobo que el pod de Odoo estaba en estado `Running` y que la aplicacion respondia desde el dominio local.

Tambien se revisaron los logs para confirmar que Odoo podia conectar con PostgreSQL.

## Prueba de escritura

Se creo un contacto de prueba desde Odoo. Esta prueba sirve para validar que:

- Odoo funciona desde navegador.
- PostgreSQL recibe y guarda los datos.
- La comunicacion interna del cluster funciona.

## Persistencia tras recrear el pod

Se elimino manualmente el pod de Odoo. Kubernetes creo un nuevo pod porque el Deployment mantiene una replica activa.

Despues de la recreacion, el contacto de prueba seguia apareciendo en Odoo. Esto confirma que el dato no dependia del contenedor eliminado.

## Caida de un nodo worker

Se apago el worker donde se estaba ejecutando Odoo. Kubernetes marco el nodo como `NotReady` y la carga acabo moviendose a otro worker disponible.

La recuperacion no fue inmediata, porque el cluster tiene que detectar la caida y Longhorn debe gestionar el volumen. Aun asi, el servicio volvio a estar disponible y los datos se conservaron.

## Almacenamiento Longhorn

Antes de la caida, los volumenes estaban en estado correcto. Tras apagar un nodo, algunas replicas quedaron afectadas y Longhorn mostro estado `degraded`.

Este estado indica perdida temporal de redundancia, no perdida directa de datos.

## Monitorizacion y alertas

Grafana se uso para revisar nodos, pods, recursos y almacenamiento. Tambien se configuraron alertas basicas para detectar:

- Nodos no disponibles.
- Pods con errores.
- Uso alto de memoria.


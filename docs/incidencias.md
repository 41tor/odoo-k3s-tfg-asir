# Incidencias

Durante el proyecto aparecieron varios problemas reales. Estos son los mas importantes.

## Multi-Attach en el volumen de Odoo

Durante una prueba de caida de nodo, Odoo quedo bloqueado por un error de tipo `Multi-Attach`.

La causa fue que el volumen estaba definido como `ReadWriteOnce`. Mientras seguia asociado al pod anterior, no podia montarse a la vez en otro nodo.

La solucion fue liberar el pod bloqueado y revisar la configuracion de Longhorn para gestionar mejor los pods asociados a nodos no disponibles.

## PostgreSQL y `lost+found`

PostgreSQL no inicializaba correctamente sobre el volumen persistente porque encontraba contenido previo en el directorio, concretamente `lost+found`.

La solucion fue definir `PGDATA` en una subcarpeta:

```yaml
env:
  - name: PGDATA
    value: /var/lib/postgresql/data/pgdata
```

Asi PostgreSQL inicializa en un directorio limpio dentro del volumen.

## Grafana sin persistencia

Al principio Grafana perdia dashboards y alertas cuando el pod se reiniciaba. El problema era que la configuracion se guardaba en almacenamiento temporal.

Se corrigio habilitando persistencia mediante un PVC de Longhorn.

## Suspension de maquinas virtuales

Suspender y reanudar las maquinas virtuales provocaba estados raros en algunos servicios, sobre todo en Longhorn y en pods dependientes de volumenes.

Para evitarlo, se opto por apagados mas controlados y por arrancar primero el nodo maestro y despues los workers.

## Apagados bruscos

Algunos apagados incorrectos generaron errores de entrada/salida en PostgreSQL. La recuperacion consistio en revisar el estado de Longhorn, reiniciar servicios afectados y esperar a que los volumenes se estabilizaran.

Esta parte dejo claro que en sistemas con almacenamiento distribuido es importante cuidar el orden de parada y arranque.

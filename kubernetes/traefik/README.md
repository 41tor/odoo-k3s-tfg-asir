# Traefik

Traefik actua como Ingress Controller del cluster.

En este proyecto se usa para publicar:

- Odoo mediante `garciaa.odoo.local`.
- Grafana mediante `garciaa.grafana.local`.

Los recursos Ingress concretos estan en las carpetas de cada servicio:

- `kubernetes/odoo/odoo-ingress.yaml`
- `kubernetes/monitorizacion/grafana-ingress.yaml`

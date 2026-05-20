# Odoo

Manifiestos y recursos relacionados con el despliegue de Odoo.

| Archivo | Contenido |
| --- | --- |
| `odoo.yaml` | Manifiesto principal usado para desplegar Odoo |
| `odoo-ingress.yaml` | Ingress para publicar Odoo mediante Traefik |
| `odoo-resources.yaml` | Recursos exportados desde el cluster real |

Odoo se despliega en el namespace `odoo` y se conecta a PostgreSQL mediante DNS interno del cluster.

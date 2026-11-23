# P09. Rol `webserver`

[Volver al índice](../../README.md)

Usar `site.yml` en esta carpeta. Ejecutar desde aquí con el inventario de la [preparación](../../00-inventario-y-claves-ssh/README.md):

```bash
ansible-playbook -i ../../00-inventario-y-claves-ssh/inventory.ini site.yml
```

**Temas:** roles, defaults, templates, handlers.

**Consigna:** convertir [P05](../P05-instalar-nginx-con-handler/README.md) y [P06](../P06-desplegar-una-pagina-web-con-template/README.md) en un rol reutilizable.

```text
roles/webserver/
├── defaults/main.yml        # web_port: 80, app_name: demo, env: dev
├── handlers/main.yml        # Reload nginx
├── tasks/main.yml           # package, template, service, firewall
└── templates/index.html.j2
```

```bash
ansible-galaxy role init roles/webserver
```

`site.yml`:

```yaml
- name: Apply webserver role
  hosts: linux
  become: true
  roles:
    - role: webserver
      vars:
        app_name: customer-api
        env: prod
```

**Preguntas:** ¿qué diferencia hay entre `defaults/` y `vars/`? ¿qué gana cuando se pasa `-e`?

## Archivos incluidos

- [`site.yml`](site.yml)

# P12. Rol `webserver`

*Temas: roles, defaults, templates, handlers.*

**Consigna:** convertir P6 + P7 en un rol reutilizable.

```
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


## Archivos de esta carpeta

- `site.yml`

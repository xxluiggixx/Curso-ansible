# P06. Desplegar una página web con template

[Volver al índice](../../README.md)

Crear `playbook.yml` en esta carpeta a partir del ejemplo. Ejecutar desde aquí con el inventario de la [preparación](../../00-inventario-y-claves-ssh/README.md):

```bash
ansible-playbook -i ../../00-inventario-y-claves-ssh/inventory.ini playbook.yml
```

**Consigna:** desplegar una página HTML usando la plantilla incluida y personalizarla con variables.

**Temas:** `template`, Jinja2, `default()`, variables, handlers.

`templates/index.html.j2`:

```html
<!DOCTYPE html>
<html>
  <head><title>{{ app_name }}</title></head>
  <body>
    <h1>{{ app_name }} - {{ env | default('dev') }}</h1>
    <p>Servidor: {{ inventory_hostname }} ({{ ansible_distribution }} {{ ansible_distribution_version }})</p>
    <p>Version: {{ app_version | default('1.0.0') }}</p>
  </body>
</html>
```

`playbook.yml`:

```yaml
- name: Deploy static website
  hosts: linux
  become: true
  vars:
    app_name: customer-api
    env: staging
    web_root: /var/www/html
  tasks:
    - name: Ensure nginx
      ansible.builtin.package:
        name: nginx
        state: present

    - name: Render index page
      ansible.builtin.template:
        src: templates/index.html.j2
        dest: "{{ web_root }}/index.html"
        owner: root
        group: root
        mode: '0644'
      notify: Reload nginx

    - name: Ensure nginx running
      ansible.builtin.service:
        name: nginx
        state: started
        enabled: true

  handlers:
    - name: Reload nginx
      ansible.builtin.service:
        name: nginx
        state: reloaded
```

## Archivos incluidos

- [`templates/index.html.j2`](templates/index.html.j2)

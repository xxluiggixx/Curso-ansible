# P04. `register` y lógica reactiva

[Volver al índice](../../README.md)

Crear `playbook.yml` en esta carpeta a partir del ejemplo. Ejecutar desde aquí con el inventario de la [preparación](../../00-inventario-y-claves-ssh/README.md):

```bash
ansible-playbook -i ../../00-inventario-y-claves-ssh/inventory.ini playbook.yml
```

**Temas:** `register`, `ignore_errors`, `when` sobre `rc`.

**Consigna:** verificar si `chronyd` está activo; si no, iniciarlo. Mostrar el contenido de la variable registrada con `debug: var=`.

```yaml
- name: Ensure service is running only if inactive
  hosts: linux
  become: true
  tasks:
    - name: Install chronyd
      ansible.builtin.package:
        name: chrony
        state: present

    - name: Check service
      ansible.builtin.command: systemctl is-active chronyd
      register: svc_status
      ignore_errors: true
      changed_when: false

    - name: Show registered result
      ansible.builtin.debug:
        var: svc_status

    - name: Start if inactive
      ansible.builtin.service:
        name: chronyd
        state: started
        enabled: true
      when: svc_status.rc != 0
```

## Verificación

```bash
ansible linux -i ../../00-inventario-y-claves-ssh/inventory.ini -b -m ansible.builtin.service -a "name=chronyd state=stopped"
```

Volver a ejecutar el playbook y comprobar que el servicio se inicia.

En sistemas donde el servicio se llama `chrony`, reemplazar `chronyd` por `chrony` al gestionar el servicio.

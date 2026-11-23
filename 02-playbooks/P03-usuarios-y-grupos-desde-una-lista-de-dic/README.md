# P03. Usuarios y grupos desde una lista de diccionarios

[Volver al índice](../../README.md)

Crear `playbook.yml` en esta carpeta a partir del ejemplo. Ejecutar desde aquí con el inventario de la [preparación](../../00-inventario-y-claves-ssh/README.md):

```bash
ansible-playbook -i ../../00-inventario-y-claves-ssh/inventory.ini playbook.yml
```

**Temas:** loop sobre dicts, `user`, `group`, `authorized_key` (opcional).

**Consigna:** crear el grupo `devs` y los usuarios `ana`, `luis` y `maria` con shell y grupo `devs`; uno de ellos debe estar `absent`.

```yaml
- name: Manage users
  hosts: linux
  become: true
  vars:
    users:
      - { name: ana,   shell: /bin/bash,   state: present }
      - { name: luis,  shell: /bin/bash,   state: present }
      - { name: maria, shell: /sbin/nologin, state: absent }
  tasks:
    - name: Ensure group devs
      ansible.builtin.group:
        name: devs
        state: present

    - name: Manage each user
      ansible.builtin.user:
        name: "{{ item.name }}"
        shell: "{{ item.shell }}"
        groups: devs
        append: true
        state: "{{ item.state }}"
        remove: "{{ item.state == 'absent' }}"
      loop: "{{ users }}"
      loop_control:
        label: "{{ item.name }}"
```

## Verificación

```bash
ssh ansible@[ip target] "sudo cat /etc/group | grep devs"
```

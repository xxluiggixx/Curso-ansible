# P03. Usuarios y grupos desde una lista de diccionarios


*Temas: loop sobre dicts, `user`, `group`, `authorized_key` (opcional).*

**Consigna:** crear el grupo `devs` y los usuarios `ana`, `luis` y `maria` con shell y grupo `devs`; uno de ellos debe estar `absent`.

```yaml
- name: Manage users
  hosts: all
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

**Verificar**

```

ssh ansible@[ip target] "sudo cat /etc/group | grep devs"
```

## Archivos de esta carpeta

- `playbook.yml`

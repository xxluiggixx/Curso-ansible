# P02. Paquetes con variables y loop

[Volver al índice](../../README.md)

Crear `playbook.yml` en esta carpeta a partir del ejemplo. Ejecutar desde aquí con el inventario de la [preparación](../../00-inventario-y-claves-ssh/README.md):

```bash
ansible-playbook -i ../../00-inventario-y-claves-ssh/inventory.ini playbook.yml
```

**Temas:** `vars`, `loop`, `package`, `become`, idempotencia.

**Consigna:** instalar la lista `packages` (`git`, `curl`, `unzip`) usando una sola task. Ejecutar dos veces y explicar `changed` vs `ok`. Luego sobrescribir la lista con `-e '{"packages": ["vim","jq"]}'`.

```yaml
- name: Install base packages
  hosts: linux
  become: true
  vars:
    packages:
      - git
      - curl
      - unzip
  tasks:
    - name: Ensure packages are present
      ansible.builtin.package:
        name: "{{ item }}"
        state: present
      loop: "{{ packages }}"
```

## Alternativa con archivo de variables

```yaml
- name: Install base packages
  hosts: linux
  become: true
  vars_files:
    - packages.yml
  tasks:
    - name: Ensure packages are present
      ansible.builtin.package:
        name: "{{ item }}"
        state: present
      loop: "{{ packages }}"
```

Crear `packages.yml` en el directorio del ejercicio:

```yaml
packages:
  - git
  - curl
  - unzip
```

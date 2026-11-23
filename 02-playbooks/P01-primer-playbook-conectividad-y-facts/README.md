# P01. Primer playbook: conectividad y facts

[Volver al índice](../../README.md)

Crear `playbook.yml` en esta carpeta a partir del ejemplo. Ejecutar desde aquí con el inventario de la [preparación](../../00-inventario-y-claves-ssh/README.md):

```bash
ansible-playbook -i ../../00-inventario-y-claves-ssh/inventory.ini playbook.yml
```

**Temas:** playbook, play, task, `ping`, `debug`, `gather_facts`.

**Consigna:** crear `playbook.yml` que (1) haga ping a todos los hosts y (2) imprima hostname, distribución y memoria.

```yaml
- name: Connectivity check and facts
  hosts: linux
  gather_facts: true
  tasks:
    - name: Ping hosts
      ansible.builtin.ping:

    - name: Print OS and memory
      ansible.builtin.debug:
        msg: "{{ inventory_hostname }} - {{ ansible_distribution }} - {{ ansible_memtotal_mb }} MB"
```

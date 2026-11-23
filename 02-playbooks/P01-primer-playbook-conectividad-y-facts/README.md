# P01. Primer playbook: conectividad y facts


*Temas: playbook, play, task, `ping`, `debug`, `gather_facts`.*

**Consigna:** crear `playbook.yml` que (1) haga ping a todos los hosts y (2) imprima hostname, distribución y memoria.

```yaml
- name: Connectivity check and facts
  hosts: all
  gather_facts: true
  tasks:
    - name: Ping hosts
      ansible.builtin.ping:

    - name: Print OS and memory
      ansible.builtin.debug:
        msg: "{{ inventory_hostname }} - {{ ansible_distribution }} - {{ ansible_memtotal_mb }} MB"
```
```bash
ansible-playbook  01-playbook.yml
```


## Archivos de esta carpeta

- `playbook.yml`

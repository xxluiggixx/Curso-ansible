# P07. Handlers y bloques `block/rescue/always`

[Volver al índice](../../README.md)

Usar `playbook.yml` en esta carpeta. Ejecutar desde aquí con el inventario de la [preparación](../../00-inventario-y-claves-ssh/README.md):

```bash
ansible-playbook -i ../../00-inventario-y-claves-ssh/inventory.ini playbook.yml
```

**Temas:** manejo de errores.

**Consigna:** intentar instalar un paquete inexistente (`paquete-que-no-existe`); si falla, registrar un mensaje con `debug` e instalar `htop` como alternativa; al final (`always`) escribir `/tmp/resultado.txt` con la fecha.

```yaml
- name: Error handling with blocks
  hosts: linux
  become: true
  tasks:
    - block:
        - name: Try to install a missing package
          ansible.builtin.package:
            name: paquete-que-no-existe
            state: present
      rescue:
        - name: Report failure
          ansible.builtin.debug:
            msg: "Fallo la instalacion en {{ inventory_hostname }}, usando alternativa"
        - name: Install fallback
          ansible.builtin.package:
            name: htop
            state: present
      always:
        - name: Write execution marker
          ansible.builtin.copy:
            content: "Ejecutado: {{ ansible_date_time.iso8601 }}\n"
            dest: /tmp/resultado.txt
            mode: '0644'
```

## Archivos incluidos

- [`playbook.yml`](playbook.yml)

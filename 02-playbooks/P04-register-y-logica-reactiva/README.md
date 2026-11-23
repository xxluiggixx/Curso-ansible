# P05. `register` y lógica reactiva

*Temas: `register`, `ignore_errors`, `when` sobre `rc`.*

**Consigna:** verificar si `chronyd` está activo; si no, iniciarlo. Mostrar el contenido de la variable registrada con `debug: var=`.

```yaml
- name: Ensure service is running only if inactive
  hosts: all
  become: true
  tasks:
    - name: Install chronyd
      ansible.builtin.package:
        name: chronyd
        state: present

    - name: Check service
      ansible.builtin.command: systemctl is-active chronyd
      register: svc_status
      ignore_errors: true
      changed_when: false

    - name: Show registered result
      ansible.builtin.debug:
        var: svc_status.rc

    - name: Start if inactive
      ansible.builtin.service:
        name: chronyd
        state: started
        enabled: true
      when: svc_status.rc != 0
```
**Detenemos el servicio**
```
ansible all -b -m service -a "name=chronyd state=stopped"
```
**Volver a ejecutar el playbook**

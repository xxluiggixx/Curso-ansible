# P06. Instalar nginx (con handler)

*Temas: `package`, `service`, handlers, `firewalld`.*

**Consigna:** instalar nginx, dejarlo habilitado e iniciado, abrir el puerto 80 y verificar con `uri` desde el control node. Reiniciar solo si cambió la configuración.

```yaml
- name: Install and configure nginx
  hosts: all
  become: true
  tasks:
    - name: Ensure nginx is present
      ansible.builtin.package:
        name: nginx
        state: present
        update_cache: true

    - name: Copy nginx tuning
      ansible.builtin.copy:
        content: "server_tokens off;\n"
        dest: /etc/nginx/conf.d/tokens.conf
        mode: '0644'
      notify: Restart nginx

    - name: Ensure nginx is started and enabled
      ansible.builtin.service:
        name: nginx
        state: started
        enabled: true

    - name: Open HTTP in firewall
      ansible.posix.firewalld:
        service: http
        permanent: true
        immediate: true
        state: enabled
      when: ansible_facts['os_family'] == "RedHat"

  handlers:
    - name: Restart nginx
      ansible.builtin.service:
        name: nginx
        state: restarted

- name: Verify from control node
  hosts: all
  gather_facts: false
  tasks:
    - name: Check HTTP 200
      ansible.builtin.uri:
        url: "http://{{ inventory_hostname }}"
        status_code: 200
      delegate_to: localhost
```
**Comprobar el handler:** correr dos veces; la segunda no debe reiniciar. Modificar el contenido y correr de nuevo.

# A07. Servicios (`service`)

**Consigna:** instalar `chrony` (o `cron`), habilitarlo al arranque y asegurarse de que esté iniciado. Después detenerlo y verificar el estado.

```bash
ansible all -b -m ansible.builtin.package -a "name=chrony state=present"
ansible all -b -m ansible.builtin.service -a "name=chronyd state=started enabled=yes"
ansible all -b -m ansible.builtin.service -a "name=chronyd state=stopped"
ansible all -m ansible.builtin.command -a "systemctl is-active chronyd"
```

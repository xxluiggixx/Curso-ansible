# A06. Servicios (`service`)

[Volver al índice](../../README.md)

Ejecutar desde esta carpeta, después de completar la [preparación](../../00-inventario-y-claves-ssh/README.md). Usar el inventario común en esta terminal:

```bash
export ANSIBLE_INVENTORY="../../00-inventario-y-claves-ssh/inventory.ini"
```

**Consigna:** instalar `chrony` (o `cron`), habilitarlo al arranque y asegurarse de que esté iniciado. Después detenerlo y verificar el estado.

```bash
ansible linux -b -m ansible.builtin.package -a "name=chrony state=present"
ansible linux -b -m ansible.builtin.service -a "name=chronyd state=started enabled=yes"
ansible linux -b -m ansible.builtin.service -a "name=chronyd state=stopped"
ansible linux -m ansible.builtin.command -a "systemctl is-active chronyd"
```

En sistemas donde el servicio se llama `chrony`, reemplazar `chronyd` por `chrony` al gestionar el servicio.

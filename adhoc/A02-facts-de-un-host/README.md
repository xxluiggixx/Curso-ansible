# A02. Facts de un host

> Ejecutar los comandos desde esta carpeta (el inventario está en `../../inventory.ini`).

**Consigna:** mostrar la distribución y la memoria; luego filtrar solo `ansible_distribution*`.

```bash
ansible all -m ansible.builtin.setup
ansible all -m ansible.builtin.setup | grep -i distri
ansible all -m ansible.builtin.setup -a "filter=ansible_distribution*"
ansible all -m ansible.builtin.setup -a "filter=ansible_memtotal_mb"

```

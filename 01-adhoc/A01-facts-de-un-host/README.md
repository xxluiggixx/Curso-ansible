# A01. Facts de un host

[Volver al índice](../../README.md)

Ejecutar desde esta carpeta, después de completar la [preparación](../../00-inventario-y-claves-ssh/README.md). Usar el inventario común en esta terminal:

```bash
export ANSIBLE_INVENTORY="../../00-inventario-y-claves-ssh/inventory.ini"
```

**Consigna:** mostrar la distribución y la memoria; luego filtrar solo `ansible_distribution*`.

```bash
ansible linux -m ansible.builtin.setup
ansible linux -m ansible.builtin.setup | grep -i distri
ansible linux -m ansible.builtin.setup -a "filter=ansible_distribution*"
ansible linux -m ansible.builtin.setup -a "filter=ansible_memtotal_mb"
```

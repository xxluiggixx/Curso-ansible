# A05. Instalar paquetes (`package`)

[Volver al índice](../../README.md)

Ejecutar desde esta carpeta, después de completar la [preparación](../../00-inventario-y-claves-ssh/README.md). Usar el inventario común en esta terminal:

```bash
export ANSIBLE_INVENTORY="../../00-inventario-y-claves-ssh/inventory.ini"
```

**Consigna:** instalar `git`, `curl` y `jq` en `linux`. Probar el modo `--check` (simulación) antes de ejecutar.

```bash
ansible linux -b -m ansible.builtin.package -a "name=git,curl,jq state=present" --check
ansible linux -b -m ansible.builtin.package -a "name=git,curl,jq state=present"
```

# A03. Copiar un archivo (`copy`) e idempotencia

[Volver al índice](../../README.md)

Ejecutar desde esta carpeta, después de completar la [preparación](../../00-inventario-y-claves-ssh/README.md). Usar el inventario común en esta terminal:

```bash
export ANSIBLE_INVENTORY="../../00-inventario-y-claves-ssh/inventory.ini"
```

**Consigna:** crear en los nodos `/tmp/bienvenida.txt` con el texto "Curso Ansible", dueño `root`, modo `0644`. Ejecutar **dos veces** y comparar `changed` vs `ok`.

```bash
ansible linux -b -m ansible.builtin.copy -a "content='Curso Ansible\n' dest=/tmp/bienvenida.txt owner=root group=root mode=0644"
# Repetir el mismo comando: debe indicar "changed": false.
ansible linux -m ansible.builtin.command -a "cat /tmp/bienvenida.txt"
```

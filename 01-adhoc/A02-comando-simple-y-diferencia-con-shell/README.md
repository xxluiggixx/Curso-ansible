# A02. Comando simple y diferencia con `shell`

[Volver al índice](../../README.md)

Ejecutar desde esta carpeta, después de completar la [preparación](../../00-inventario-y-claves-ssh/README.md). Usar el inventario común en esta terminal:

```bash
export ANSIBLE_INVENTORY="../../00-inventario-y-claves-ssh/inventory.ini"
```

**Consigna:** obtener `uptime` y `df -h /` de todos los linux. Después probar un pipe (`ps aux | wc -l`) con `command` y con `shell`.

```bash
ansible linux -m ansible.builtin.command -a "uptime"
ansible linux -m ansible.builtin.command -a "df -h /"
ansible linux -m ansible.builtin.command -a "ps aux | wc -l"   # falla
ansible linux -m ansible.builtin.shell   -a "ps aux | wc -l"   # funciona
```

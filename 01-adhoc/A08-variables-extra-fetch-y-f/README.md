# A08. Variables extra, `fetch` y `-f`

[Volver al índice](../../README.md)

Ejecutar desde esta carpeta, después de completar la [preparación](../../00-inventario-y-claves-ssh/README.md). Usar el inventario común en esta terminal:

```bash
export ANSIBLE_INVENTORY="../../00-inventario-y-claves-ssh/inventory.ini"
```

**Consigna:** traer `/etc/hostname` de cada nodo al control node (`/tmp/hostnames/`), ejecutando con 1 fork y luego con 5 (`-f`).

```bash
ansible linux -m ansible.builtin.fetch -a "src=/etc/hostname dest=/tmp/hostnames/" -f 1
ansible linux -m ansible.builtin.fetch -a "src=/etc/hostname dest=/tmp/hostnames/" -f 5
```

Para parametrizar el destino con una variable extra:

```bash
ansible linux -m ansible.builtin.fetch \
  -a 'src=/etc/hostname dest={{ destino }}' \
  -e 'destino=/tmp/hostnames/' -f 5
```

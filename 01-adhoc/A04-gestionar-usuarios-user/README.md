# A04. Gestionar usuarios (`user`)

[Volver al índice](../../README.md)

Ejecutar desde esta carpeta, después de completar la [preparación](../../00-inventario-y-claves-ssh/README.md). Usar el inventario común en esta terminal:

```bash
export ANSIBLE_INVENTORY="../../00-inventario-y-claves-ssh/inventory.ini"
```

**Consigna:** crear el usuario `pepe` con shell `/bin/bash` y grupo adicional `wheel` (Ubuntu: `sudo`); verificar con `id`; luego eliminarlo con su home.

```bash
openssl passwd -6 '1234'
ansible linux -b -m ansible.builtin.user \
  -a 'name=pepe shell=/bin/bash groups=sudo append=yes state=present password=[Copy hash]'
ansible linux -m ansible.builtin.command -a "id pepe"
ansible linux -b -m ansible.builtin.user -a "name=pepe state=absent remove=yes"
```

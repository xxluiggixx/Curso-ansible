# A07. Archivos y directorios (`file`, `lineinfile`, `stat`)

[Volver al índice](../../README.md)

Ejecutar desde esta carpeta, después de completar la [preparación](../../00-inventario-y-claves-ssh/README.md). Usar el inventario común en esta terminal:

```bash
export ANSIBLE_INVENTORY="../../00-inventario-y-claves-ssh/inventory.ini"
```

**Consigna:**

1. Crear `/opt/curso` con modo `0755` y dueño `pepe`.
2. Agregar la línea `ENV=dev` a `/opt/curso/app.env` (sin duplicarla al repetir).
3. Comprobar que existe con `stat`.

El usuario `pepe` debe existir; si se eliminó en A04, volver a crearlo antes de continuar.

```bash
ansible linux -b -m ansible.builtin.file -a "path=/opt/curso state=directory mode=0755 owner=pepe"
ansible linux -b -m ansible.builtin.lineinfile \
  -a "path=/opt/curso/app.env line='ENV=dev' create=yes"
ansible linux -m ansible.builtin.stat -a "path=/opt/curso/app.env"
```

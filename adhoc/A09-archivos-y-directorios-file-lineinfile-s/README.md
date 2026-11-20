# A09. Archivos y directorios (`file`, `lineinfile`, `stat`)

**Consigna:**
1. Crear `/opt/curso` con modo `0755` y dueño `pepe`.
2. Agregar la línea `ENV=dev` a `/opt/curso/app.env` (sin duplicarla al repetir).
3. Comprobar que existe con `stat`.

```bash
ansible all -b -m ansible.builtin.file -a "path=/opt/curso state=directory mode=0755 owner=pepe"
ansible all -b -m ansible.builtin.lineinfile \
  -a "path=/opt/curso/app.env line='ENV=dev' create=yes"
ansible all -m ansible.builtin.stat -a "path=/opt/curso/app.env"
```

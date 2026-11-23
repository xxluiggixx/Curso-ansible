# A05. Gestionar usuarios (`user`)

**Consigna:** crear el usuario `pepe` con shell `/bin/bash` y grupo adicional `wheel` (Ubuntu: `sudo`); verificar con `id`; luego eliminarlo con su home.

```bash
openssl passwd -6 '1234'
ansible all -b -m ansible.builtin.user \
  -a 'name=pepe shell=/bin/bash groups=sudo append=yes state=present password=[Copy hash]'
ansible all -m ansible.builtin.command -a "id pepe"
ansible all -b -m ansible.builtin.user -a "name=pepe state=absent remove=yes"
```

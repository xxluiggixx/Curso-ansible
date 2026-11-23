# A06. Instalar paquetes (`package`)


**Consigna:** instalar `git`, `curl` y `jq` en `linux`. Probar el modo `--check` (simulación) antes de ejecutar.

```bash

ansible all -b -m ansible.builtin.package -a "name=firewalld state=present update_cache=true" --check
ansible all -b -m ansible.builtin.package -a "name=firewalld state=present update_cache=true"
ansible all -b -m ansible.builtin.package -a "name=git,curl state=present update_cache=true"
```
# A10. Variables extra, `fetch` y `-f`

**Consigna:** traer `/etc/hostname` de cada nodo al control node (`/tmp/hostnames/`), ejecutando con 1 fork y luego con 5 (`-f`).

```bash
ansible all -m ansible.builtin.fetch \
  -a "src=/etc/hostname dest=." 
```

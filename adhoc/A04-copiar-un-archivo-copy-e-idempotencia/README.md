# A04. Copiar un archivo (`copy`) e idempotencia


**Consigna:** crear en los nodos `/tmp/bienvenida.txt` con el texto "Curso Ansible", dueño `root`, modo `0644`. Ejecutar **dos veces** y comparar `changed` vs `ok`.

```bash
ansible all -b -m ansible.builtin.copy -a "content='Curso Ansible\n' dest=/tmp/bienvenida.txt owner=root group=root mode=0644"
# repetir el mismo comando: debe salir SUCCESS sin "changed"
ansible all -m ansible.builtin.command -a "cat /tmp/bienvenida.txt"
```


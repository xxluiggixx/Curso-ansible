# A03. Comando simple y diferencia con `shell`

**Consigna:** obtener `uptime` y `df -h /` de todos los linux. Después probar un pipe (`ps aux | wc -l`) con `command` y con `shell`.

```bash
ansible all -m ansible.builtin.command -a "uptime"
ansible all -m ansible.builtin.command -a "df -h /"
ansible all -m ansible.builtin.command -a "ps aux | wc -l"   # falla
ansible all -m ansible.builtin.shell   -a "ps aux | wc -l"   # funciona
```
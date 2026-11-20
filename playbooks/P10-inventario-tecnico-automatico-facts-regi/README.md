# P10. Inventario técnico automático (facts + `register`)

*Temas: facts, `template`, `delegate_to`, `lineinfile`/`copy`.*

**Consigna:** generar en el control node un archivo CSV `/tmp/inventario_tecnico.csv` con una línea por host: `hostname,ip,distribucion,ram_mb,cpus`. Pista: usar `template` con `delegate_to: localhost`, `run_once: true` y un `for` sobre `ansible_play_hosts` con `hostvars`.

```jinja
hostname,ip,distribucion,ram_mb,cpus
{% for h in ansible_play_hosts %}
{{ h }},{{ hostvars[h].ansible_default_ipv4.address }},{{ hostvars[h].ansible_distribution }},{{ hostvars[h].ansible_memtotal_mb }},{{ hostvars[h].ansible_processor_vcpus }}
{% endfor %}
```

```
- name: Generate technical inventory
  hosts: all
  gather_facts: true

  tasks:
    - name: Generate CSV on control node
      ansible.builtin.template:
        src: templates/inventario_tecnico.csv.j2
        dest: ./inventario_tecnico.csv
        mode: '0644'
      delegate_to: localhost
      run_once: true
```

## Archivos de esta carpeta

- `templates/inventario.csv.j2`

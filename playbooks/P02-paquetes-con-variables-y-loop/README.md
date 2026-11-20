# P02. Paquetes con variables y loop

*Temas: `vars`, `loop`, `package`, `become`, idempotencia.*

**Consigna:** instalar la lista `packages` (`git`, `curl`, `unzip`) usando una sola task. Ejecutar dos veces y explicar `changed` vs `ok`. Luego sobrescribir la lista con `-e '{"packages": ["vim","jq"]}'`.

```yaml
- name: Install base packages
  hosts: all
  become: true
  vars:
    packages:
      - git
      - curl
      - unzip
  tasks:
    - name: Ensure packages are present
      ansible.builtin.package:
        name: "{{ item }}"
        state: present
      loop: "{{ packages }}"
```

**Alternativa con file var**

```yaml
- name: Install base packages
  hosts: all
  become: true
  vars_files:
    - packages.yml
  tasks:
    - name: Ensure packages are present
      ansible.builtin.package:
        name: "{{ item }}"
        state: present
      loop: "{{ packages }}"
```
Crear en el directorio el archivo packages.yml
```yaml

packages:
  - git
  - curl
  - unzip

```

## Archivos de esta carpeta

- `playbook.yml`

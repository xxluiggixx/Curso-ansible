# 00. Definir el inventario y conectar con par de claves SSH

> Ejercicio previo a todos los demás. Se ejecuta desde esta carpeta; al terminar, el inventario resultante queda en `../inventory.ini` y lo usan el resto de los ejercicios.

**Objetivo:** dejar el *control node* listo para administrar las VMs: definir el inventario, generar un par de claves SSH, copiar la clave pública a cada VM con `ssh-copy-id` y comprobar la conexión con Ansible, sin contraseña.

**Temas del temario:** control node, managed nodes, inventory (grupos y `ansible_host`), agentless por SSH, comando ad-hoc `ping`.

## Escenario

| Rol | Nombre | IP |
|---|---|---|
| Control node | `control` | (donde corre Ansible) |
| Managed node | `app01` | (ejemplo)10.10.1.11 |

## Paso 1. Crear la carpeta de trabajo

```bash
mkdir -p ~/curso-ansible && cd ~/curso-ansible
```

## Paso 2. Generar el par de claves SSH

Se genera una clave dedicada para Ansible (tipo `ed25519`, más moderna y corta que RSA).

```bash
ssh-keygen 
```
## Paso 3. Copiar la clave pública a cada VM con `ssh-copy-id`

```bash
ssh-copy-id ansible@10.10.1.11
```

La primera vez aparece:

```
The authenticity of host '10.10.1.11' can't be established.
Are you sure you want to continue connecting (yes/no)? yes
ansible@10.10.1.11's password: ansible
```

## Paso 4. Probar el acceso por clave (sin contraseña)

```bash
ssh  ansible@10.10.1.11 hostname
```

## Paso 5. Crear el inventario estático (formato INI)

Crear el archivo `inventory.ini` en la carpeta de trabajo:

### Ejemplo

```ini
[linux]
app01 ansible_host=10.10.1.11
app02 ansible_host=10.10.1.12

[windows]
win01 ansible_host=10.10.2.21

[linux:vars]
ansible_user=ansible
ansible_ssh_private_key_file=~/.ssh/id_ansible
```

## Paso 6. Primera conexión con Ansible (`ping`)

```bash
ansible all -i inventory.ini -m ansible.builtin.ping
```

Salida esperada:

```
app01 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
app02 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

Otras pruebas:

```bash
ansible all -i inventory.ini -m ansible.builtin.setup          # un solo host
ansible all -i inventory.ini -m ansible.builtin.command -a "hostname"
ansible all -i inventory.ini -b -m ansible.builtin.command -a "whoami"   # debe decir root
```

## Paso 7. Evitar repetir `-i` con `ansible.cfg`

Crear `ansible.cfg` en la carpeta de trabajo:

```ini
[defaults]
inventory = ./inventory.ini
host_key_checking = False
remote_user = ansible

[privilege_escalation]
become_method = sudo
```

Ahora basta con:

```bash
ansible all -m ansible.builtin.ping
```


## Paso 8. (Opcional) Inventario en formato YAML

Equivalente al INI. Archivo [`inventory.yml`](inventory.yml):

```yaml
all:
  children:
    linux:
      hosts:
        app01:
          ansible_host: 10.10.1.11
      vars:
        ansible_user: ansible
        ansible_ssh_private_key_file: ~/.ssh/id_ansible

```
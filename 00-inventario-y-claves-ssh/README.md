# 00. Definir el inventario y conectar con par de claves SSH

[Volver al índice](../README.md)

**Objetivo:** preparar el nodo de control para administrar las máquinas del laboratorio por SSH con un par de claves.

**Temas:** nodo de control, nodos administrados, inventario, grupos, `ansible_host`, SSH y módulo `ping`.

## Requisitos

- Ansible y un cliente SSH instalados en el nodo de control.
- Máquinas Linux accesibles por red, con SSH habilitado.
- Un usuario remoto con permisos de `sudo` para los ejercicios que usan `become`.

## Escenario

| Rol | Nombre | Dirección |
| --- | --- | --- |
| Nodo de control | `control` | Máquina desde la que se ejecuta Ansible |
| Nodo administrado | `app01` | `10.10.1.11` (ejemplo) |
| Nodo administrado | `app02` | `10.10.1.12` (ejemplo) |

Reemplazar las direcciones y el usuario `ansible` por los de tu laboratorio.

## Paso 1. Entrar en la carpeta de preparación

Desde la raíz del repositorio:

```bash
cd 00-inventario-y-claves-ssh
```

El inventario común queda en esta carpeta. Los demás ejercicios lo referencian mediante su ruta relativa.

## Paso 2. Generar el par de claves SSH

Crear una clave dedicada, con el mismo nombre que usa el inventario:

```bash
ssh-keygen 
```


## Paso 3. Copiar la clave pública a los nodos

```bash
ssh-copy-id  ansible@10.10.1.11
```

En la primera conexión se solicita confirmar la identidad del host y escribir la contraseña del usuario remoto.

## Paso 4. Probar el acceso por clave

```bash
ssh  ansible@10.10.1.11 hostname
```

## Paso 5. Adaptar el inventario INI

Editar el archivo [`inventory.ini`](inventory.ini). Para los ejercicios Linux, la configuración de ejemplo es:

```ini
[linux]
app01 ansible_host=10.10.1.11
app02 ansible_host=10.10.1.12

[linux:vars]
ansible_user=ansible
ansible_ssh_private_key_file=~/.ssh/id_ansible
```

El archivo incluido también contiene un grupo `windows`. Los comandos de esta guía se limitan a `linux`.

## Paso 6. Comprobar la conexión con Ansible

```bash
ansible linux -i inventory.ini -m ansible.builtin.ping
```

Cada nodo debe devolver `SUCCESS`, con `"changed": false` y `"ping": "pong"`.

Otras comprobaciones:

```bash
ansible app01 -i inventory.ini -m ansible.builtin.setup
ansible linux -i inventory.ini -m ansible.builtin.command -a "hostname"
ansible linux -i inventory.ini -b -m ansible.builtin.command -a "whoami"
```

El último comando debe devolver `root`. Si `sudo` requiere contraseña, agregar `-K`.

## Paso 7. Usar la configuración incluida

El archivo [`ansible.cfg`](ansible.cfg) define `./inventory.ini` como inventario y `ansible` como usuario remoto. Desde esta carpeta se puede ejecutar:

```bash
ansible linux -m ansible.builtin.ping
```

Al cambiar a otra carpeta, seguir las instrucciones de inventario de cada ejercicio: Ansible no busca este archivo de configuración en las carpetas superiores.

## Paso 8. Probar el inventario YAML (opcional)

El archivo [`inventory.yml`](inventory.yml) ofrece otro ejemplo de inventario. Adaptarlo a los mismos nodos antes de usarlo:

```bash
ansible linux -i inventory.yml -m ansible.builtin.ping
```

## Archivos incluidos

- [`inventory.ini`](inventory.ini): inventario común del curso.
- [`inventory.yml`](inventory.yml): ejemplo en formato YAML.
- [`ansible.cfg`](ansible.cfg): configuración para esta carpeta.

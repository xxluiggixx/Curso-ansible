# P09. Rol `webserver`

[Volver al índice](../../README.md)

**Consigna resuelta:** convertir [P05](../P05-instalar-nginx-con-handler/README.md) y [P06](../P06-desplegar-una-pagina-web-con-template/README.md) en un rol reutilizable.

**Temas:** roles, defaults, templates, handlers y precedencia de variables.

## Qué hace la solución

1. Instala Nginx y crea el directorio de la página.
2. Genera el HTML con el nombre de la aplicación, entorno, versión y datos del host.
3. Configura el puerto HTTP y desactiva `server_tokens`. Valida la configuración con `nginx -t` antes de reemplazarla.
4. Habilita e inicia Nginx. Recarga el servicio mediante un handler cuando cambia su configuración; cambiar solamente el HTML no requiere recarga.
5. En la familia Red Hat, instala y habilita `firewalld` y abre el puerto configurado de forma inmediata y permanente.
6. Aplica los handlers pendientes y verifica desde el nodo de control la respuesta HTTP 200 y el contenido de la página.

El rol administra `/etc/nginx/nginx.conf` completo y conserva una copia al modificarlo. Está pensado para las máquinas dedicadas del laboratorio, sin sitios Nginx previos que deban mantenerse.

## Archivos incluidos

```text
.
├── requirements.yml
├── site.yml
└── roles/webserver/
    ├── defaults/main.yml
    ├── handlers/main.yml
    ├── tasks/main.yml
    └── templates/
        ├── index.html.j2
        └── nginx.conf.j2
```

- [`site.yml`](site.yml): aplica el rol al grupo `linux`, con `customer-api` y `prod`.
- [`requirements.yml`](requirements.yml): colección `ansible.posix`, utilizada para el firewall.
- [`defaults/main.yml`](roles/webserver/defaults/main.yml): variables configurables.
- [`tasks/main.yml`](roles/webserver/tasks/main.yml): instalación, configuración y comprobaciones.
- [`handlers/main.yml`](roles/webserver/handlers/main.yml): recarga de Nginx.
- [`index.html.j2`](roles/webserver/templates/index.html.j2): página web.
- [`nginx.conf.j2`](roles/webserver/templates/nginx.conf.j2): configuración del servidor.

El rol ya está creado; no hace falta ejecutar `ansible-galaxy role init`.

## Requisitos

Completar la [preparación del inventario y SSH](../../00-inventario-y-claves-ssh/README.md). Los nodos deben ser Linux con Python, `sudo`, un gestor de servicios y repositorios que proporcionen Nginx. El nodo de control necesita Ansible y acceso de red al puerto HTTP de cada nodo.

En Debian/Ubuntu el rol no modifica UFW ni otros firewalls. Si están activos, permitir el puerto HTTP. En sistemas con SELinux, un puerto o directorio personalizado debe tener una política/contexto que permita su uso por Nginx; el rol no modifica SELinux.

## Ejecución

Desde la raíz del repositorio:

```bash
cd 02-playbooks/P09-rol-webserver
ansible-galaxy collection install -r requirements.yml
ansible-playbook -i ../../00-inventario-y-claves-ssh/inventory.ini site.yml --syntax-check
ansible-playbook -i ../../00-inventario-y-claves-ssh/inventory.ini site.yml
```

Agregar `-K` si `sudo` requiere contraseña. Para ejecutar sobre una sola máquina, agregar `--limit app01`.

## Variables

| Variable | Valor por defecto | Uso |
| --- | --- | --- |
| `web_port` | `80` | Puerto HTTP, entre 1 y 65535 |
| `web_root` | `/usr/share/nginx/html` | Directorio de la página |
| `app_name` | `demo` | Nombre de la aplicación |
| `env` | `dev` | Entorno mostrado |
| `app_version` | `1.0.0` | Versión mostrada |
| `web_manage_firewall` | `true` en Red Hat; `false` en otras familias | Gestionar `firewalld` |

`site.yml` sobrescribe `app_name` y `env`. Para personalizar la ejecución:

```bash
ansible-playbook -i ../../00-inventario-y-claves-ssh/inventory.ini site.yml \
  -e '{"app_name": "mi-web", "env": "staging", "app_version": "2.0.0"}'
```

También se puede pasar `-e '{"web_port": 8080}'` si el firewall y SELinux permiten ese puerto. Al cambiarlo, el rol abre el nuevo puerto en `firewalld`, pero no elimina reglas anteriores. Si el firewall se administra por otro medio, usar `-e '{"web_manage_firewall": false}'`.

## Comprobación e idempotencia

Ejecutar dos veces el mismo comando. En la segunda ejecución, con el estado sin cambios y la caché de paquetes vigente, se espera `changed=0` y ninguna recarga de Nginx. Cambiar `app_name` modifica la página; cambiar `web_port` modifica la configuración y dispara el handler.

El rol comprueba automáticamente la página usando `ansible_host` (o el nombre del inventario si no está definido). También se puede abrir `http://IP_DEL_NODO:80/` en un navegador.

Después de un primer despliegue, se pueden revisar cambios con:

```bash
ansible-playbook -i ../../00-inventario-y-claves-ssh/inventory.ini site.yml --check --diff
```

La comprobación HTTP se omite en modo check. Una simulación sobre un nodo nuevo puede fallar porque Nginx y sus directorios todavía no están instalados: `--check` no instala los paquetes.

## Respuestas del ejercicio

- **`defaults/` frente a `vars/`:** `defaults/main.yml` contiene valores de baja prioridad, fáciles de sobrescribir desde el inventario o el playbook. `vars/main.yml` tiene mayor prioridad que las variables del inventario y del play, por lo que no conviene usarlo para estas opciones configurables.
- **¿Qué gana con `-e`?** Las variables extra tienen la mayor prioridad. Por ejemplo, `-e 'env=staging'` reemplaza tanto `dev` de los defaults como `prod` de `site.yml`.

Referencia: [precedencia de variables de Ansible](https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_variables.html).

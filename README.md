# Fundamentos de Ansible

Ejercicios prácticos para aprender inventarios, comandos ad-hoc y playbooks.

## Cómo usar este repositorio

1. Completar la preparación y adaptar el [inventario común](00-inventario-y-claves-ssh/inventory.ini) a los hosts del laboratorio.
2. Recorrer los ejercicios ad-hoc (A01–A08) y luego los playbooks (P01–P09).
3. Ejecutar los comandos desde la carpeta de cada ejercicio, siguiendo su README. Los ejemplos se centran en el grupo `linux`.

Los bloques de código forman parte de las consignas. Cuando un archivo aún no existe, el ejercicio indica que hay que crearlo. La sección «Archivos incluidos» enlaza archivos presentes en el repositorio.

## Índice

### Preparación

- [00. Definir el inventario y conectar con par de claves SSH](00-inventario-y-claves-ssh/README.md)

### Comandos ad-hoc

- [A01. Facts de un host](01-adhoc/A01-facts-de-un-host/README.md)
- [A02. Comando simple y diferencia con `shell`](01-adhoc/A02-comando-simple-y-diferencia-con-shell/README.md)
- [A03. Copiar un archivo (`copy`) e idempotencia](01-adhoc/A03-copiar-un-archivo-copy-e-idempotencia/README.md)
- [A04. Gestionar usuarios (`user`)](01-adhoc/A04-gestionar-usuarios-user/README.md)
- [A05. Instalar paquetes (`package`)](01-adhoc/A05-instalar-paquetes-package/README.md)
- [A06. Servicios (`service`)](01-adhoc/A06-servicios-service/README.md)
- [A07. Archivos y directorios (`file`, `lineinfile`, `stat`)](01-adhoc/A07-archivos-y-directorios-file-lineinfile-s/README.md)
- [A08. Variables extra, `fetch` y `-f`](01-adhoc/A08-variables-extra-fetch-y-f/README.md)

### Playbooks

- [P01. Primer playbook: conectividad y facts](02-playbooks/P01-primer-playbook-conectividad-y-facts/README.md)
- [P02. Paquetes con variables y loop](02-playbooks/P02-paquetes-con-variables-y-loop/README.md)
- [P03. Usuarios y grupos desde una lista de diccionarios](02-playbooks/P03-usuarios-y-grupos-desde-una-lista-de-dic/README.md)
- [P04. `register` y lógica reactiva](02-playbooks/P04-register-y-logica-reactiva/README.md)
- [P05. Instalar nginx (con handler)](02-playbooks/P05-instalar-nginx-con-handler/README.md)
- [P06. Desplegar una página web con template](02-playbooks/P06-desplegar-una-pagina-web-con-template/README.md)
- [P07. Handlers y bloques `block/rescue/always`](02-playbooks/P07-handlers-y-bloques-block-rescue-always/README.md)
- [P08. Inventario técnico automático (facts + `register`)](02-playbooks/P08-inventario-tecnico-automatico-facts-regi/README.md)
- [P09. Rol `webserver`](02-playbooks/P09-rol-webserver/README.md)

### Material complementario

- [Inventario de ejemplo en la raíz](example.inventory.ini).
- [Inventario en YAML](00-inventario-y-claves-ssh/inventory.yml).
- [Configuración de Ansible](00-inventario-y-claves-ssh/ansible.cfg).
- [Ejemplo de actualización de paquetes](Ejemplos/system-update-packages.yml).
- [Archivo de mantenimiento de Docker](Ejemplos/docker-maintenance.yml): actualmente contiene HTML guardado de GitHub, pendiente de reemplazar por el YAML correspondiente.

## Relación entre temas y ejercicios

| Tema | Ejercicios |
| --- | --- |
| Inventario, SSH y conectividad | 00, P01 |
| Facts | A01, P01, P08 |
| Comandos y shell | A02 |
| Archivos e idempotencia | A03, A07, P02 |
| Usuarios y grupos | A04, P03 |
| Paquetes y servicios | A05, A06, P02, P04, P05 |
| Variables y loops | A08, P02, P03, P06 |
| `register` y condicionales | P04 |
| Handlers | P05, P06 |
| Bloques `block`, `rescue` y `always` | P07 |
| Templates Jinja2 | P06, P08 |
| Roles | P09 |

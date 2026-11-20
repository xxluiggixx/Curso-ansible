# Ejercicios — Fundamentos de Ansible

Inventario común: [`inventory.ini`](inventory.ini). Cada carpeta tiene su `README.md` con la consigna.


## Preparación (hacer primero)

- [00. Definir el inventario y conectar con par de claves SSH](00-inventario-y-claves-ssh/README.md)

## Ad-hoc

- [A01. Conectividad](adhoc/A01-conectividad/README.md)
- [A02. Facts de un host](adhoc/A02-facts-de-un-host/README.md)
- [A03. Comando simple y diferencia con `shell`](adhoc/A03-comando-simple-y-diferencia-con-shell/README.md)
- [A04. Copiar un archivo (`copy`) e idempotencia](adhoc/A04-copiar-un-archivo-copy-e-idempotencia/README.md)
- [A05. Gestionar usuarios (`user`)](adhoc/A05-gestionar-usuarios-user/README.md)
- [A06. Instalar paquetes (`package`)](adhoc/A06-instalar-paquetes-package/README.md)
- [A07. Servicios (`service`)](adhoc/A07-servicios-service/README.md)
- [A08. Firewall (`ansible.posix.firewalld`)](adhoc/A08-firewall-ansible-posix-firewalld/README.md)
- [A09. Archivos y directorios (`file`, `lineinfile`, `stat`)](adhoc/A09-archivos-y-directorios-file-lineinfile-s/README.md)
- [A10. Variables extra, `fetch` y `-f`](adhoc/A10-variables-extra-fetch-y-f/README.md)
- [A11. Windows (opcional)](adhoc/A11-windows-opcional/README.md)

## Playbooks

- [P01. Primer playbook: conectividad y facts](playbooks/P01-primer-playbook-conectividad-y-facts/README.md)
- [P02. Paquetes con variables y loop](playbooks/P02-paquetes-con-variables-y-loop/README.md)
- [P03. Usuarios y grupos desde una lista de diccionarios](playbooks/P03-usuarios-y-grupos-desde-una-lista-de-dic/README.md)
- [P04. Condicionales por sistema operativo](playbooks/P04-condicionales-por-sistema-operativo/README.md)
- [P05. `register` y lógica reactiva](playbooks/P05-register-y-logica-reactiva/README.md)
- [P06. Instalar nginx (con handler)](playbooks/P06-instalar-nginx-con-handler/README.md)
- [P07. Desplegar una página web con template](playbooks/P07-desplegar-una-pagina-web-con-template/README.md)
- [P08. Archivo de configuración con template (`.conf.j2`)](playbooks/P08-archivo-de-configuracion-con-template-co/README.md)
- [P09. Handlers y bloques `block/rescue/always`](playbooks/P09-handlers-y-bloques-block-rescue-always/README.md)
- [P10. Inventario técnico automático (facts + `register`)](playbooks/P10-inventario-tecnico-automatico-facts-regi/README.md)
- [P11. Mantenimiento de sistema (similar al playbook de la carpeta)](playbooks/P11-mantenimiento-de-sistema-similar-al-play/README.md)
- [P12. Rol `webserver`](playbooks/P12-rol-webserver/README.md)
- [P13. Collections y Windows (opcional)](playbooks/P13-collections-y-windows-opcional/README.md)

## Proyecto final

- [Proyecto final](proyecto-final/README.md)

## Mapa ejercicio ↔ temario

| Tema del temario | Ejercicios |
|---|---|
| Arquitectura / ad-hoc / inventario | A1, A2, A3 |
| Módulos básicos | A4–A10 |
| Playbook, play, task | P1, P2 |
| Variables y loops | P2, P3, P8 |
| Facts y `register` | P1, P4, P5, P10 |
| Condicionales | P4, P5, P11 |
| Handlers | P6, P7, P8 |
| Blocks (`rescue`/`always`) | P9 |
| Templates Jinja2 | P7, P8, P10 |
| Roles | P12, Proyecto final |
| Collections / FQCN | A8, A11, P13 |

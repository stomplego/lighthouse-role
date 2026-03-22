# Ansible Role: Lighthouse

[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-lighthouse--role-blue.svg)](https://galaxy.ansible.com/stomplego/lighthouse-role)

## Описание

Роль для установки и настройки Lighthouse — веб-интерфейса для мониторинга ClickHouse.

## Требования

- Ansible >= 2.9
- Ubuntu 20.04 / 22.04 / 24.04
- Минимум 100 МБ свободного места

## Переменные роли

| Имя | Значение по умолчанию | Описание |
|-----|----------------------|----------|
| `lighthouse_version` | `master` | Версия/тег репозитория |
| `lighthouse_install_dir` | `/var/www/lighthouse` | Директория установки |
| `lighthouse_nginx_port` | `8080` | Порт для доступа |
| `lighthouse_nginx_server_name` | `localhost` | Server name для nginx |
| `lighthouse_repo_url` | `https://github.com/VKCOM/lighthouse.git` | URL репозитория |

## Пример playbook

### Базовая установка

```yaml
---
- name: Установка Lighthouse
  hosts: all
  become: yes
  roles:
    - role: stomplego.lighthouse-role

### Переопределение переменных

```yaml
---
- hosts: monitoring_servers
  roles:
    - role: lighthouse-role
      vars:
        lighthouse_nginx_port: 9090
        lighthouse_install_dir: /opt/lighthouse

### Шаблоны
Роль включает шаблон lighthouse.conf.j2 в директории templates/:

```nginx
---
server {
    listen {{ lighthouse_nginx_port }};
    server_name {{ lighthouse_nginx_server_name }};
    location / {
        root {{ lighthouse_install_dir }};
        index index.html;
    }
}

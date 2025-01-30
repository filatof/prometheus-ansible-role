# Ansible Роль: Установка Prometheus

## Описание
Эта Ansible-роль устанавливает и настраивает Prometheus на целевых серверах. При наличии Consul используется соответствующий шаблон конфигурации Prometheus.

## Требования
Для использования этой роли необходимо установить её через Ansible Galaxy:

```bash
ansible-galaxy install -p roles -r roles/requirements.yml
```

Файл `roles/requirements.yml` должен содержать:

```yaml
- src: https://github.com/filatof/prometheus-ansible-role.git
  name: prometheus
  scm: git
  version: main
```

## Переменные роли
Роль поддерживает следующие переменные, которые можно настроить:

- `prometheus_version`: "3.0.1" – версия Prometheus.
- `prometheus_force_install`: `false` – принудительная переустановка Prometheus.
- `prometheus_config_dir`: `/etc/prometheus` – каталог конфигурации Prometheus.
- `prometheus_data_dir`: `/var/lib/prometheus` – каталог данных Prometheus.
- `prometheus_web_listen_address`: `"0.0.0.0:9090"` – адрес и порт, на котором работает Prometheus.
- `prometheus_web_external_url`: `""` – внешний URL Prometheus.
- `prometheus_storage_retention`: `"30d"` – срок хранения метрик.
- `prometheus_storage_retention_size`: `"0"` – лимит хранения данных (0 – без ограничения).
- `prometheus_config_flags_extra`: `{}` – дополнительные флаги конфигурации.
- `prometheus_alertmanager_config`: `[]` – настройки интеграции с Alertmanager.

## Логика установки
### Конфигурация Prometheus:
- Если на целевой машине установлен Consul, используется шаблон конфигурации с autodiscovery через Consul.
- Если Consul отсутствует, применяется стандартный шаблон конфигурации с определёнными метками и настройками алертинга.

### Основные задачи:
- Создание systemd unit-файла для Prometheus.
- Добавление alerting-правил через шаблоны Ansible.
- Копирование пользовательских файлов правил алертинга.
- Добавление основного конфигурационного файла `prometheus.yml` с валидацией через `promtool`.

## Пример использования
Пример использования роли в playbook:

```yaml
- hosts: all
  become: yes
  roles:
    - role: prometheus
      prometheus_version: "3.0.1"
      prometheus_web_listen_address: "0.0.0.0:9090"
      prometheus_storage_retention: "15d"
```

## Лицензия
BSD

## Автор
Разработчик: filatof EQ
Репозиторий: [GitHub](https://github.com/filatof/prometheus-ansible-role)


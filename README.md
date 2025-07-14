
# Система мониторинга с Docker Compose и сбором логов через Ansible

## Компоненты
- Prometheus для сбора метрик с сервиса логов
- Grafana для визуализации метрик
- Filebeat (локальный systemd-сервис) для сбора логов контейнеров и записи в /var/log/test

## Развертывание

### 1. Запуск мониторинга (Docker Compose)
```bash
cd monitoring
docker-compose up -d


## Проверка

- Grafana доступна: `http://<IP>:80`
- Prometheus на http://<IP>:9090

2. Установка сервиса сбора логов
cd monitoring/ansible
ansible-playbook playbook.yml

Сервис filebeat запущен и собирает логи из контейнеров в /var/log/test

Метрики, собираемые Prometheus:

filebeat_events_total — количество обработанных логов по контейнерам
В Grafana можно использовать Explore для поиска метрик по названию
Проверка

Логи контейнеров должны появляться в /var/log/test с указанием контейнера
В Grafana на дашборде "Logs Metrics" должны отображаться метрики из Prometheus


## Стек:
- Docker Compose: Grafana + Prometheus
- Ansible: Установка Filebeat на хост
- Filebeat: tail логов Docker, запись в файл, отдача метрик Prometheus
=======


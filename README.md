# Monitoring with Grafana + Prometheus + Fluentd

## Установка

```bash
cd monitoring && docker-compose up -d
cd ../ansible && ansible-playbook playbook.yml
```

## Проверка

- Grafana доступна: `http://<IP>:80`
- В Grafana → Explore найдите метрики: `fluentd_output_status_emit_count`, `fluentd_input_status_emit_count`
- Логи пишутся в `/var/log/test`

## Стек:
- Docker Compose: Grafana + Prometheus
- Ansible: Установка Fluentd на хост
- Fluentd: tail логов Docker, запись в файл, отдача метрик Prometheus

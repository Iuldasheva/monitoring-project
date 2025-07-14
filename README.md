Monitoring Stack: Grafana + Prometheus + Filebeat

Полноценная система мониторинга и сбора логов контейнеров Docker.
📦 Состав
Prometheus — сбор метрик
Grafana — визуализация метрик
Filebeat — сбор логов контейнеров и экспорт метрик в Prometheus
Ansible — установка Filebeat как systemd-сервиса (вне Docker)

Структура проекта

project-root/
├── docker/
│   ├── docker-compose.yml
│   ├── prometheus/
│   │   └── prometheus.yml
│   └── filebeat/
│       └── filebeat.yml
├── ansible/
│   ├── playbook.yml
│   └── files/
│       └── filebeat.yml
└── README.md

⚙️ Требования

Ubuntu 24.04
Docker
Docker Compose
Ansible
🚀 Этапы развёртывания

1️⃣ Склонируйте или скачайте проект
git clone https://github.com/your-username/monitoring-project.git
cd monitoring-project/docker

2️⃣ Запустите стек мониторинга (Prometheus + Grafana + Filebeat в Docker)
docker compose up -d
⏱ Через несколько секунд запустятся:

Prometheus (http://<IP>:9090)
Grafana (http://<IP>:80)
Filebeat экспортирует метрики на http://<IP>:5066/metrics

3️⃣ Установите Filebeat как systemd-сервис (вне Docker)
Перейдите в папку Ansible:
cd ../ansible
Запустите playbook:
ansible-playbook playbook.yml

После этого:

Filebeat будет установлен (если ещё не установлен)
Конфигурация /etc/filebeat/filebeat.yml будет заменена
Сервис filebeat будет перезапущен
Логи контейнеров будут записываться в /var/log/test/container-logs.log

📈 Интерфейсы и доступы

Компонент	URL	Логин / Пароль
Grafana	http://<IP-адрес-хоста>:80	admin / admin
Prometheus	http://<IP-адрес-хоста>:9090	—
Filebeat	http://<IP-адрес-хоста>:5066/metrics	(Prometheus target)

📊 Настройка Grafana

Зайдите в Grafana по адресу: http://<IP>:80
Перейдите:
⚙️ Configuration → Data Sources → Add data source
Выберите Prometheus
Введите URL:
http://prometheus:9090
Нажмите Save & Test

▶️ Создание панели в Dashboard
Перейдите в + → Dashboard → Add new panel
Выберите Data Source: Prometheus
Введите запрос:
rate(filebeat_events_total[1m])
Нажмите Apply
Вы увидите количество логов, обрабатываемых Filebeat в секунду.

📋 Полезные команды

Проверить статус Filebeat:
sudo systemctl status filebeat
Следить за логами Filebeat:
sudo tail -f /var/log/test/container-logs.log
Проверить, что метрики Filebeat доступны:
curl http://localhost:5066/metrics



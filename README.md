# Домашнее задание к занятию "Docker. Часть 2" - `Kravchenko V.A.`

### Задание 1

`Напишите ответ в свободной форме, не больше одного абзаца текста.

Установите Docker Compose и опишите, для чего он нужен и как может улучшить лично вашу жизнь.`

### Решение 1. 
* Docker Compose - это инструмент для определения и запуска многоконтейнерных приложений Docker с помощью удобного файла конфигурации YAML. Он позволяет легко настроить, запускать и масштабировать приложения, обеспечивая их надежность и изоляцию. Docker Compose упрощает разработку, тестирование и развертывание приложений, уменьшая сложность инфраструктуры и сокращая время настройки окружения, что в свою очередь делает жизнь проще и продуктивнее. Docker Compose мою личную жизнь только усложнил.
![Скриншот 1 к заданию 1](https://github.com/keebertron/Docker2-Kravchenko-V.A./blob/main/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%201%20%D0%BA%20%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D1%8E%201.png)


### Задание 2

Выполните действия и приложите текст конфига на этом этапе.

Создайте файл docker-compose.yml и внесите туда первичные настройки:

version;
services;
volumes;
networks.
При выполнении задания используйте подсеть 10.5.0.0/16. Ваша подсеть должна называться: <ваши фамилия и инициалы>-my-netology-hw. Все приложения из последующих заданий должны находиться в этой конфигурации.

### Решение 2

```
version: '3'

services:
  netology-app

volumes:
  netology-lesson:
    6-04_docker2

networks:
  kravchenko_va-my-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16
          gateway: 10.5.0.1
````
---


### Задание 3

**Выполните действия:** 

1. Создайте конфигурацию docker-compose для Prometheus с именем контейнера <ваши фамилия и инициалы>-netology-prometheus. 
2. Добавьте необходимые тома с данными и конфигурацией (конфигурация лежит в репозитории в директории [6-04/prometheus](https://github.com/netology-code/sdvps-homeworks/tree/main/lecture_demos/6-04/prometheus) ).
3. Обеспечьте внешний доступ к порту 9090 c докер-сервера.

### Решение 3

```
Поле для вставки кода...
# my global config
global:
  scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
            # - alertmanager:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
   The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
   - job_name: "Kravchenko_VA-netology-prometheus"
     metrics_path defaults to '/metrics'
     scheme defaults to 'http'.
     static_configs:
       - targets: ["10.5.0.0:9090"]

#  - job_name: 'pushgateway'
#    honor_labels: true
#    static_configs:
#      - targets: ["pushgateway:9091"]

```


### Задание 4

`Выполните действия:

Создайте конфигурацию docker-compose для Pushgateway с именем контейнера <ваши фамилия и инициалы>-netology-pushgateway.
Обеспечьте внешний доступ к порту 9091 c докер-сервера.`

### Решение 4


```

version: '3'

services:
  pushgateway:
    image: prom/pushgateway:v1.6.2
    container_name:kravchenko_va-netology-prometheus
    ports:
      - 9091:9091
    networks:
      - monitoring-stack
    depends_on:
      - prometheus
    restart: unless-stopped

```


### Задание 5

Выполните действия:

Создайте конфигурацию docker-compose для Grafana с именем контейнера <ваши фамилия и инициалы>-netology-grafana.
Добавьте необходимые тома с данными и конфигурацией (конфигурация лежит в репозитории в директории 6-04/grafana).
Добавьте переменную окружения с путем до файла с кастомными настройками (должен быть в томе), в самом файле пропишите логин=<ваши фамилия и инициалы> пароль=netology.
Обеспечьте внешний доступ к порту 3000 c порта 80 докер-сервера.`

1. `Заполните здесь этапы выполнения, если требуется ....`
2. `Заполните здесь этапы выполнения, если требуется ....`
3. `Заполните здесь этапы выполнения, если требуется ....`
4. `Заполните здесь этапы выполнения, если требуется ....`
5. `Заполните здесь этапы выполнения, если требуется ....`
6. 
### Решение 5
```
Поле для вставки кода...
....
....
....
....
```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота](ссылка на скриншот)`


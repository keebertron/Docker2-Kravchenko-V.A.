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
  KravchenkoVA-my-netology-hw:
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

version: '3'
services:
  prometheus:
    image: prom/prometheus:v2.47.2
    container_name: KravchenkoVA-prometheus
    command: --web.enable-lifecycle --config.file=/etc/prometheus/prometheus.yml
    ports:
      - 9090:9090
    volumes:
      - ./prometheus:/etc/prometheus
      - prometheus-data:/prometheus
    networks:
      - KravchenkoVA-my-netology-hw
    restart: always

volumes:
  prometheus-data:

```


### Задание 4

`Выполните действия:

Создайте конфигурацию docker-compose для Pushgateway с именем контейнера <ваши фамилия и инициалы>-netology-pushgateway.
Обеспечьте внешний доступ к порту 9091 c докер-сервера.`

### Решение 4


```
version: '3'

services:
  prometheus:
    image: prom/prometheus:v2.47.2
    container_name: KravchenkoVA-prometheus
    command: --web.enable-lifecycle --config.file=/etc/prometheus/prometheus.yml
    ports:
      - 9090:9090
    volumes:
      - ./prometheus:/etc/prometheus
      - prometheus-data:/prometheus
    networks:
      - KravchenkoVA-my-netology-hw
    restart: always

  pushgateway:
    image: prom/pushgateway:v1.6.2
    container_name: KravchenkoVA-pushgateway
    ports:
      - 9091:9091
    networks:
      - KravchenkoVA-my-netology-hw
    depends_on:
      - prometheus
    restart: unless-stopped

volumes:
  prometheus-data:

networks:
  KravchenkoVA-my-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16
          gateway: 10.5.0.1

```
![Скриншот 1 к заданию 4](https://github.com/keebertron/Docker2-Kravchenko-V.A./blob/main/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%201%20%D0%BA%20%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D1%8E%204.png)

### Задание 5

Выполните действия:

Создайте конфигурацию docker-compose для Grafana с именем контейнера <ваши фамилия и инициалы>-netology-grafana.
Добавьте необходимые тома с данными и конфигурацией (конфигурация лежит в репозитории в директории 6-04/grafana).
Добавьте переменную окружения с путем до файла с кастомными настройками (должен быть в томе), в самом файле пропишите логин=<ваши фамилия и инициалы> пароль=netology.
Обеспечьте внешний доступ к порту 3000 c порта 80 докер-сервера.`
 
### Решение 5
```
services:
  prometheus:
    image: prom/prometheus:v2.47.2
    container_name: KravchenkoVA-prometheus
    command: --web.enable-lifecycle --config.file=/etc/prometheus/prometheus.yml
    ports: 
      - 9090:9090
    volumes:
      - ./prometheus:/etc/prometheus
      - prometheus-data:/prometheus
    networks:
      - KravchenkoVA-my-netology-hw
    restart: always

  pushgateway:
    image: prom/pushgateway:v1.6.2
    container_name: KravchenkoVA-pushgateway
    ports:
      - 9091:9091
    networks:
      - KravchenkoVA-my-netology-hw
    depends_on:
      - prometheus
    restart: unless-stopped

  grafana:
    image: grafana/grafana
    container_name: KravchenkoVA-grafana
    environment:
      GF_PATHS_CONFIG: /etc/grafana/custom.ini
    ports:
      - 80:3000
    volumes:
      - ./grafana:/etc/grafana
      - grafana-data:/var/lib/grafana
    networks:
      - KravchenkoVA-my-netology-hw
    depends_on:
      - prometheus
    restart: unless-stopped
 
volumes:
  prometheus-data:
  grafana-data:
  
networks:
  KravchenkoVA-my-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16
          gateway: 10.5.0.1

```
custom.ini
```
[security]

admin_user = KravchenkoVA
admin_password = netology

```
### Задание 6

Выполните действия.

1. Настройте поочередность запуска контейнеров.
2. Настройте режимы перезапуска для контейнеров.
3. Настройте использование контейнерами одной сети.
4. Запустите сценарий в detached режиме.

### Решение 6
```
services:
  prometheus:
    restart: always
    networks:
KravchenkoVA-my-netology-hw
  pushgateway:
    depends_on: prometheus
    restart: unless-stopped
   networks: KravchenkoVA-my-netology-hw
   grafana:
    depends_on: pushgateway
    restart: unless-stopped
    networks: KravchenkoVA-my-netology-hw
```
![Скриншот 1 к заданию 6](https://github.com/keebertron/Docker2-Kravchenko-V.A./blob/main/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%201%20%D0%BA%20%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D1%8E%206.png)

 ### Задание 7
 Выполните действия.

1. Выполните запрос в Pushgateway для помещения метрики <ваши фамилия и инициалы> со значением 5 в Prometheus: echo "<ваши фамилия и инициалы> 5" | curl --data-binary @- http://localhost:9091/metrics/job/netology.
Залогиньтесь в Grafana с помощью логина и пароля из предыдущего задания.
2. Cоздайте Data Source Prometheus (Home -> Connections -> Data sources -> Add data source -> Prometheus -> указать "Prometheus server URL = http://prometheus:9090" -> Save & Test).
3. Создайте график на основе добавленной в пункте 5 метрики (Build a dashboard -> Add visualization -> Prometheus -> Select metric -> Metric explorer -> <ваши фамилия и инициалы -> Apply.
В качестве решения приложите:

docker-compose.yml целиком;
скриншот команды docker ps после запуске docker-compose.yml;
скриншот графика, постоенного на основе вашей метрики.

### Решение 7
```
echo "KravchenkoVA 5" | curl --data-binary @- http://localhost:9091/metrics/job/netology
```
![Скриншот 1 к заданию 7](https://github.com/keebertron/Docker2-Kravchenko-V.A./blob/main/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%201%20%D0%BA%20%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D1%8E%207.png)

![Скриншот 2 к заданию 7](https://github.com/keebertron/Docker2-Kravchenko-V.A./blob/main/%D0%A1%D0%BA%D1%80%D0%B8%D0%BD%D1%88%D0%BE%D1%82%201%20%D0%BA%20%D0%B7%D0%B0%D0%B4%D0%B0%D0%BD%D0%B8%D1%8E%207.png)

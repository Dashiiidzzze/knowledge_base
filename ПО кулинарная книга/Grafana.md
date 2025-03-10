Для подключения **Grafana** к **Prometheus** нужно сделать следующее:

---

## Шаг 1: Настройка Prometheus

Убедитесь, что **Prometheus** настроен и собирает метрики вашего приложения. В файле `prometheus.yml` определите ваш **job** (задачу):

```yaml
global:
  scrape_interval: 10s # Интервал сбора метрик

scrape_configs:
  - job_name: "cookbook_backend" # Название задачи
    static_configs:
      - targets: ["web:8080"] # Замените на адрес вашего сервера
```

- Здесь `web:8080` — это адрес вашего приложения, где доступны метрики (например, через `/metrics`).

**Запуск Prometheus**:

Если используете **Docker Compose**, добавьте сервис Prometheus в ваш `docker-compose.yml`:

```yaml
prometheus:
  image: prom/prometheus:latest
  container_name: prometheus
  volumes:
    - ./prometheus.yml:/etc/prometheus/prometheus.yml # Монтируем конфиг
  ports:
    - "9090:9090" # Пробрасываем порт для доступа к Prometheus
  restart: always
```

**Проверка**: Запустите Docker Compose и откройте `http://localhost:9090`. Убедитесь, что Prometheus видит ваши метрики.

---

## Шаг 2: Добавление Grafana в проект

Добавьте сервис **Grafana** в `docker-compose.yml`:

```yaml
grafana:
  image: grafana/grafana:latest
  container_name: grafana
  ports:
    - "3000:3000" # Пробрасываем порт для доступа к Grafana
  volumes:
    - grafana_data:/var/lib/grafana # Хранение данных Grafana
  environment:
    - GF_SECURITY_ADMIN_USER=admin     # Логин администратора
    - GF_SECURITY_ADMIN_PASSWORD=admin # Пароль администратора
  depends_on:
    - prometheus # Grafana запустится после Prometheus
  restart: always

volumes:
  grafana_data: # Том для хранения данных Grafana
```

**Запуск**: Перезапустите Docker Compose:

```bash
docker-compose up -d
```

---

## Шаг 3: Подключение Prometheus в Grafana

1. **Откройте Grafana**:  
    Перейдите на `http://localhost:3000`.  
    Логин и пароль по умолчанию:
    
    - **User**: `admin`
    - **Password**: `admin` (или ваш пароль из `docker-compose.yml`).
2. **Добавьте источник данных**:
    
    - В левой панели Grafana нажмите **Configuration** → **Data Sources**.
    - Нажмите **Add data source**.
    - Выберите **Prometheus**.
    - В поле **URL** укажите адрес Prometheus: `http://prometheus:9090`.
        - Если Grafana и Prometheus работают в одной сети Docker Compose, используйте имя сервиса (например, `prometheus`).
    - Нажмите **Save & Test**, чтобы проверить подключение.

---

## Шаг 4: Настройка дашбордов

1. **Импорт готовых дашбордов**:
    
    - В Grafana перейдите в **Dashboards** → **Import**.
    - Можно загрузить дашборды с сайта [Grafana Dashboards](https://grafana.com/grafana/dashboards/):
        - Например, готовый дашборд для Prometheus: **ID 1860** (Node Exporter Full).
    - В поле **Import via grafana.com** введите ID и нажмите **Load**.
2. **Создайте свой дашборд**:
    
    - Нажмите **+** → **Dashboard** → **Add a new panel**.
    - В панели введите PromQL-запросы для ваших метрик.
    
    **Примеры запросов**:
    
    - **Доступность БД**:
        
        ```promql
        db_up
        ```
        
    - **Время выполнения SQL-запросов**:
        
        ```promql
        histogram_quantile(0.95, sum(rate(db_query_duration_seconds_bucket[5m])) by (le))
        ```
        
    - **HTTP-запросы**:
        
        ```promql
        http_requests_total
        ```
        
3. **Сохраните дашборд**:  
    Нажмите **Save** и задайте имя вашему дашборду.
    

---

## Шаг 5: Проверка работы

1. **Убедитесь, что Grafana подключена к Prometheus** и видит метрики.
2. Откройте дашборд и проверьте отображение данных.

---

### Итоговая структура Docker Compose:

Ваш файл `docker-compose.yml` должен выглядеть так:

```yaml
version: '3.8'

services:
  web:
    build:
      context: ./backend
    ports:
      - "8080:8080"
    environment:
      - API_PORT=8080
    depends_on:
      - db

  db:
    image: postgres:alpine
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: cookbook
    ports:
      - "5432:5432"
    volumes:
      - db_data:/var/lib/postgresql/data

  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    ports:
      - "3000:3000"
    volumes:
      - grafana_data:/var/lib/grafana
    environment:
      - GF_SECURITY_ADMIN_USER=admin
      - GF_SECURITY_ADMIN_PASSWORD=admin
    depends_on:
      - prometheus

volumes:
  db_data:
  grafana_data:
```

---

## Результат

1. **Prometheus** собирает метрики с вашего приложения.
2. **Grafana** подключена к Prometheus и визуализирует данные.
3. Доступ к интерфейсам:
    - **Prometheus**: [http://localhost:9090](http://localhost:9090/)
    - **Grafana**: [http://localhost:3000](http://localhost:3000/)

Теперь можно отслеживать состояние вашего приложения и базы данных в реальном времени! 🚀
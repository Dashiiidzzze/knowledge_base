Чтобы проверить состояние `healthcheck` уже запущенного контейнера, используйте команду `docker inspect`. Она позволяет получить подробную информацию о контейнере, включая статус его healthcheck.

Вот как это сделать:

### Шаги:

1. **Используйте команду `docker ps`**, чтобы получить список запущенных контейнеров:
    
    ```bash
    docker ps
    ```
    
    Эта команда отобразит список всех работающих контейнеров, их ID и имена. Найдите нужный контейнер по имени или ID.
    
2. **Используйте команду `docker inspect`**, чтобы получить информацию о контейнере, включая статус его `healthcheck`:
    
    ```bash
    docker inspect --format='{{json .State.Health}}' <container_id_or_name>
    ```
    
    Замените `<container_id_or_name>` на ID или имя контейнера, статус которого вы хотите проверить. Например:
    
    ```bash
    docker inspect --format='{{json .State.Health}}' my_container_name
    ```
    
    Это покажет статус healthcheck в JSON-формате, например:
    
    ```json
    {
      "Status": "healthy",
      "FailingStreak": 0,
      "Log": [
        {
          "Start": "2024-12-19T14:10:31.5994218Z",
          "End": "2024-12-19T14:10:34.0508775Z",
          "ExitCode": 0,
          "Output": "Health check passed"
        },
        ...
      ]
    }
    ```
    
    В этом выводе:
    
    - **Status**: текущее состояние контейнера (например, `healthy`, `unhealthy`, `starting`).
    - **FailingStreak**: количество неудачных попыток healthcheck подряд.
    - **Log**: журнал попыток проверки состояния (включает время, код завершения и вывод команды).
3. **Для более подробного вывода** можно использовать `docker inspect` без фильтра:
    
    ```bash
    docker inspect <container_id_or_name>
    ```
    
    Это выведет всю информацию о контейнере, включая статус его `healthcheck` в разделе `State` → `Health`.
    

### Пример вывода:

```json
"State": {
  "Status": "running",
  "Running": true,
  "Paused": false,
  "Restarting": false,
  "OOMKilled": false,
  "Dead": false,
  "Pid": 12345,
  "ExitCode": 0,
  "Error": "",
  "StartedAt": "2024-12-19T14:05:23.5432105Z",
  "FinishedAt": "0001-01-01T00:00:00Z",
  "Health": {
    "Status": "healthy",
    "FailingStreak": 0,
    "Log": [
      {
        "Start": "2024-12-19T14:10:31.5994218Z",
        "End": "2024-12-19T14:10:34.0508775Z",
        "ExitCode": 0,
        "Output": "Health check passed"
      }
    ]
  }
}
```

Здесь вы видите, что состояние контейнера — `healthy`, и в журнале записана информация о последней успешной проверке.

Теперь вы можете отслеживать здоровье контейнера и быстро реагировать на возможные проблемы.
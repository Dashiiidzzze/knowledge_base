**Переменные окружения** — это значения, которые используются в различных командах и программных сценариях, выполняемых в операционной системе. Представляют пары «ключ-значение» и используются для хранения параметров, настроек приложений, хранения ключей и других информационных данных.

можно посмотреть командой `printenv`

https://habr.com/ru/companies/gnivc/articles/792082/ инфа про .env

### .env.example
Правилами хорошего тона считается создание файла с именем **.env.example**, который должен хранить список необходимых для заполнения переменных. Этот файл используется для документирования и предоставления возможного примера заполнения значений переменных окружения.



пример файла .env
DB_HOST=db
DB_PORT=5431
DB_USER=dashidze
DB_PASSWORD=das10002
DB_NAME=cookbook
API_PORT=8080


пример использования в docker-compose.yml
```yml
services:
	web:
		build:
			context: ./backend # Путь к Dockerfile
		ports:
			- "${API_PORT}:${API_PORT}" # Проброс порта для веб-приложения
		volumes:
			- ./config:/app/config # монтируем папку для просмотра внутри контейнера
		environment:
			- DB_HOST=${DB_HOST}
			- DB_PORT=${DB_PORT}
			- DB_USER=${DB_USER}
			- DB_PASSWORD=${DB_PASSWORD}
			- DB_NAME=${DB_NAME}
			- API_PORT=${API_PORT}
		depends_on:
			- db # Сначала запускается база данных
	db:
		image: postgres:alpine # Готовый образ PostgreSQL для Alpine
		environment:
			POSTGRES_USER: ${DB_USER} # Пользователь базы данных
			POSTGRES_PASSWORD: ${DB_PASSWORD} # Пароль пользователя
			POSTGRES_DB: ${DB_NAME} # Имя базы данных
		ports:
			- "${DB_PORT}:5432" # Проброс порта для PostgreSQL

		volumes:
			- db_data:/var/lib/postgresql/data # Том для хранения данных
			- ./initdb:/docker-entrypoint-initdb.d # все скрипты из этой папки должны выполняться
		restart: always # Перезапуск при сбое

volumes:
	db_data: # Том для базы данных
```


посмотреть можно командой docker-compose config

FROM alpine
или
FROM openjdk:17-jdk-slim-buster - образ:версия
WORKDIR /app - папка для хранения информации внутри образа
COPY /build/libs/Proga.jar /app/proga.jar - наш файл и куда его надо скопировать. относительно докерфайла
ENTRYPOINT ["java", "-jar", "proga.jar"] - команды для запуска программы

docker build -t proga . - создание образа. нужно находится в папке с докерфайлом.

```Dockerfile
# Используем базовый образ Alpine с Go
FROM golang:1.22.2-alpine

# Устанавливаем необходимые зависимости
RUN apk add --no-cache git curl build-base

# Создаём рабочую директорию внутри контейнера
WORKDIR /app

# Копируем файлы зависимостей (go.mod и go.sum) и устанавливаем их
COPY go.mod ./
#COPY go.sum ./
RUN go mod download

# Копируем весь код приложения в контейнер
COPY . ./

# Собираем бинарный файл приложения
RUN go build ./cmd/main.go

# Указываем порт, который будет использовать приложение
EXPOSE 8080

# Запускаем приложение
CMD ["./main"] 
```

Каждая новая строка это новый слой контейнера! Чем меньше слоев, тем лучше.
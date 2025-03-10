FROM alpine
или
FROM openjdk:17-jdk-slim-buster - образ:версия
WORKDIR /app - папка для хранения информации внутри образа
COPY /build/libs/Proga.jar /app/proga.jar - наш файл и куда его надо скопировать. относительно докерфайла
ENTRYPOINT ["java", "-jar", "proga.jar"] - команды для запуска программы

docker build -t proga . - создание образа. нужно находится в папке с докерфайлом.
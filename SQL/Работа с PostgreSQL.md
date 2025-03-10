запуск базы данных в контейнере: [[docker-compose.yml]]

`sudo -i -u postgres` - запуск пользователя постгрес
`exit` - выход из пользователя постгрес

`postgres@dasha-Modern-14-C5M:~$ man psql` - мануал

`postgres@dasha-Modern-14-C5M:~$ psql` - открываем консоль постгрес
`postgres=# \l `- просмотр существующих бд
`postgres=# \q` - выход из консоли постгрес
`postgres=# \du` - вывод всех пользователей
`postgres=# CREATE USER dashidze WITH PASSWORD 'das10002'; `- создание пользователя
`postgres=# DROP USER dashidze;` - удаление пользователя
`postgres=# ALTER USER dashidze WITH SUPERUSER;` - выдача суперпользователя

`postgres@dasha-Modern-14-C5M:~$ createdb < имя >` - создание бд
`postgres@dasha-Modern-14-C5M:~$ dropdb < имя >` - удаление бд

`postgres@dasha-Modern-14-C5M:~$ psql -U dashidze -d cookbook -h 127.0.0.1 -W` - подключение к консоли бд из под другого пользователя


## создание бд
1) `sudo -i -u postgres` - запуск пользователя постгрес
2) `postgres=# CREATE USER dashidze WITH PASSWORD 'das10002';` - создание пользователя
3) `postgres@dasha-Modern-14-C5M:~$ createdb < имя >` - создание бд
4) `postgres@dasha-Modern-14-C5M:~$ psql -U dashidze -d cookbook -h 127.0.0.1 -W` - подключение к консоли бд из под другого пользователя
5) втыкаем нашу бд в консоль в виде:
```SQL
-- Таблица пользователей
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    username VARCHAR(100) UNIQUE NOT NULL,
    password_hash TEXT NOT NULL
);

-- Таблица рецептов
CREATE TABLE recipes (
    id SERIAL PRIMARY KEY,
    user_id INT REFERENCES users(id) ON DELETE CASCADE,
    name VARCHAR(255) NOT NULL,
    dish_type VARCHAR(100),
    cook_time INTERVAL,
    is_favorite BOOLEAN DEFAULT FALSE,
    holiday VARCHAR(100),
    instructions TEXT,
    photo BYTEA
);

-- Таблица ингредиентов
CREATE TABLE ingredients (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL UNIQUE
);

-- Таблица связи рецептов и ингредиентов
CREATE TABLE recipe_ingredients (
    id SERIAL PRIMARY KEY,
    recipe_id INT REFERENCES recipes(id) ON DELETE CASCADE,
    ingredient_id INT REFERENCES ingredients(id) ON DELETE CASCADE,
    quantity VARCHAR(100)
);
```
6) `cookbook=# \dt` - просмотр чего создалось
7) попроверять разными инсертами и селектами
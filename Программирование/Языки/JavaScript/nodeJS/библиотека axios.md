Axios в Node.js — **это HTTP-клиент, основанный на промисах, который упрощает выполнение HTTP-запросов**. Он оптимизирует такие задачи, как отправка GET и POST запросов, обработка ответов и управление ошибками.

Axios автоматически преобразует данные JSON как для запросов, так и для ответов, устраняя необходимость в ручном парсинге.

Инициализация проекта: `npm init -y`
Установка: `npm install axios`
Подключение в коде: `const axios = require('axios');`

### Выполнение GET-запросов

GET-запрос извлекает данные из указанной конечной точки. Вот как выполнить GET-запрос с помощью Axios:

```js
const axios = require('axios');

async function fetchData() {
  try {
    const response = await axios.get('https://jsonplaceholder.typicode.com/posts/1');
    console.log(response.data);
  } catch (error) {
    console.error('Ошибка при получении данных:', error);
  }
}

fetchData();
```

#### Объяснение:

- Определяет асинхронную функцию `fetchData`.
- Использует `await` для выполнения GET-запроса через Axios.
- Выводит полученные данные в консоль.
- Обрабатывает ошибки с помощью `try...catch`.

### Выполнение POST-запросов

POST-запрос отправляет данные на сервер для создания нового ресурса. Вот как выполнить POST-запрос:

```js
const axios = require('axios');

async function createPost() {
  try {
    const response = await axios.post('https://jsonplaceholder.typicode.com/posts', {
      title: 'Новый пост',
      body: 'Это содержимое нового поста.',
      userId: 1,
    });
    console.log('Пост создан:', response.data);
  } catch (error) {
    console.error('Ошибка при создании поста:', error);
  }
}

createPost();
```

#### Объяснение:

- Определяет асинхронную функцию `createPost`.
- Использует `await` с `axios.post()`, передавая конечную точку и объект данных.
- Выводит созданный пост.
- Корректно обрабатывает ошибки.

### Обработка ответов и ошибок

Axios предоставляет структурированный способ обработки ответов и ошибок. Объект ответа включает такие свойства, как `data`, `status` и `headers`.

Вот как обрабатывать различные типы ошибок:

```js
const axios = require('axios');

async function fetchData() {
  try {
    const response = await axios.get('https://jsonplaceholder.typicode.com/posts/1');
    console.log('Данные:', response.data);
    console.log('Статус:', response.status);
  } catch (error) {
    if (error.response) {
      // Сервер ответил кодом состояния вне диапазона 2xx
      console.error('Данные ошибки:', error.response.data);
      console.error('Статус ошибки:', error.response.status);
    } else if (error.request) {
      // Ответ не получен
      console.error('Ответ не получен:', error.request);
    } else {
      // Ошибка при настройке запроса
      console.error('Ошибка:', error.message);
    }
  }
}

fetchData();
```

### Объяснение:

- Выводит данные и статус ответа при успешном выполнении.
- Различает:
    - Ошибки с ответами (`error.response`)
    - Ошибки без ответа (`error.request`)
    - Другие ошибки, связанные с Axios (`error.message`)

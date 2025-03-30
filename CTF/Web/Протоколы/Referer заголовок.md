Заголовок запроса **`Referer`** содержит URL исходной страницы, с которой был осуществлён переход на текущую страницу. Заголовок `Referer` позволяет серверу узнать откуда был осуществлён переход на запрашиваемую страницу. Сервер может анализировать эти данные, записывать их в логи или оптимизировать процесс кеширования.

пример использования
``` HTTP
GET / HTTP/1.1
Host: natas4.natas.labs.overthewire.org
Cache-Control: max-age=0
Authorization: Basic bmF0YXM0OlFyeVpYYzJlMHphaFVMZEhydEh4enlZa2o1OWtVeExR
Accept-Language: ru-RU
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.6533.100 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate, br
Referer: http://natas5.natas.labs.overthewire.org/
Connection: keep-alive
```

используем здесь `Referer: http://natas5.natas.labs.overthewire.org/` чтобы создать видимость перехода с другой страницы.
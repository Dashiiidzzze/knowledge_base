Basic authentication - базовая аутентификация доступа.

В контексте HTTP транзакции, базовая аутентификация доступа - это метод, с помощью которого пользовательский агент HTTP (например, веб-браузер) предоставляет имя пользователя и пароль при отправке запроса. В базовой HTTP-аутентификации запрос содержит поле заголовка в форме Authorization: Basic <credentials>, где <credentials> - это кодировка ID и пароля Base64, соединенные одним двоеточием :.

пример:

	GET /second/level20/ HTTP/1.1
	Connection: close
	Host: kslweb1.spb.ctf.su
	Authorization: Basic YWRtaW46czNjcjN0cDQkJHcwUkQ=

здесь ID - admin, пароль - s3cr3tp4$$w0RD

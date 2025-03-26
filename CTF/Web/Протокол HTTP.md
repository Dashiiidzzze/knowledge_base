## HTML
как посмотреть сайт изнутри:
- html разметка (View page source - посмотреть код страницы) не совпадает с кодом, который отрисовывается в окне браузера. Так как подгружаются доп данные и выполняются скрипты
- Inspect - исследовать элемент - показывает код, который существует в данный момент.

HTTP протокол - правила на основе которых формируется запрос к сайту.

**GET** -  метод для чтения данных с сайта. Например, для доступа к указанной странице. Mожно послать параметры как http://kslweb1.spb.ctf.su/first/level7?login=dasha. Аналогично их можно отправить через GET /blog/?name=Dasha HTTP/1.1

	GET /blog/ HTTP/1.1					- то, что мы собстренно хотим от сайта (GET /url адрес/ протокол/версия протокола)
	Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7     - определяет, какие типы данных может обработать клиент;
	Accept-Encoding: gzip, deflate				- обозначает форматы и кодировку файлов
	Accept-Language: ru,en;q=0.9				- указывает на принимаемые языки;
	Cache-Control: max-age=0
	Connection: keep-alive
	Host: hax.tor.hu					        - по какому хосту мы идем на сайт
	If-Modified-Since: Sat, 10 Feb 2024 17:06:31 GMT
	If-None-Match: "6a-6110a10edc18e-gzip"
	Upgrade-Insecure-Requests: 1
	User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/124.0.0.0 YaBrowser/24.6.0.0 Safari/537.36 	- мой юзерэджнт
	Cookie: yuidss=3178431581697901949			- как нас отличить от других

**POST** - метод для отправки данных на сайт. Чаще всего с помощью метода POST передаются формы. Параметы можно передавать через специальные тулзы в теле HTTP запроса, но и как в гет запросе.

	POST /soeasy/ HTTP/2
	Host: intro.spbctf.com
	Cache-Control: max-age=0
	Sec-Ch-Ua: "Chromium";v="127", "Not)A;Brand";v="99"
	Sec-Ch-Ua-Mobile: ?0
	Sec-Ch-Ua-Platform: "Linux"
	Sec-Ch-Ua-Platform-Version: ""
	Sec-Ch-Ua-Full-Version-List: 
	Accept-Language: ru-RU
	Upgrade-Insecure-Requests: 1
	User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.6533.89 Safari/537.36
	Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
	Sec-Fetch-Site: none
	Sec-Fetch-Mode: navigate
	Sec-Fetch-User: ?1
	Sec-Fetch-Dest: document
	Accept-Encoding: gzip, deflate, br
	Priority: u=0, i
	Content-Type: application/x-www-form-urlencoded   - тип передаваемых данных
	Content-Length: 7                                 - длина передаваемых данных
	
	fake=no                                           - сами данные

в POST запросе можно отправлять файлы: [[MIME_опр_типа_файла]].

Когда браузер видит [[cookies]] он записывает их к тебе и будет передавать сайту при каждом твоем входе.

в ответ на запрос приходит [[status code (код ошибки)]].
все передаваемые параметры(данные) кодируются [[стандарт кодирования url]].
запросы можно принимать и отправлять при помощи [[утилита curl]]





левое окно:
Elements - текущий html документ
Console - отображаются отладочные сообщения
Sources - все картинки и скрипты странички
Network - все что делает браузер при загрузке страницы. Можно отсортировать и потыкать на документы.
Timeline - инфа в какое время какой запрос был послан.
Application - отображает куки




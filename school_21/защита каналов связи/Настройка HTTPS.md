### **Запуск и проверка NGINX**
`sudo systemctl start nginx` - запуск
`sudo systemctl enable nginx` - запуск при старте VM
`sudo systemctl status nginx` - проверка статуса - должно быть active (running)

Адрес готовой страницы: http://localhost

### **Генерация SSL/TLS сертификатов (самоподписанных)**
`sudo mkdir -p /etc/nginx/ssl` - Создание директории для сертификатов
`sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/ssl/nginx-selfsigned.key -out /etc/nginx/ssl/nginx-selfsigned.crt` - Генерация ключа и самоподписанного сертификата
	**При запросе данных (Subject) заполняйте так:**
	- **Country Name (2 letter code):** RU
	- **State or Province Name:** (можно оставить пустым — нажать Enter)
	- **Locality Name:** (можно оставить пустым)
	- **Organization Name:** (например, MyCompany)
	- **Organizational Unit Name:** (например, IT)
	- **Common Name (важно!):** `localhost` (или IP-адрес вашей виртуальной машины)
	- **Email Address:** (можно оставить пустым)

`sudo openssl dhparam -out /etc/nginx/ssl/dhparam.pem 2048` - Создание группированного файла Diffie-Hellman (для усиления безопасности)

### **Настройка NGINX для работы по HTTPS**
`sudo nano /etc/nginx/sites-available/https-site` - Создание конфигурационного файла для HTTPS-сайта
Конфигурация:
```nginx
server {
    listen 443 ssl default_server;
    listen [::]:443 ssl default_server;

    server_name localhost;

    ssl_certificate /etc/nginx/ssl/nginx-selfsigned.crt;
    ssl_certificate_key /etc/nginx/ssl/nginx-selfsigned.key;
    ssl_dhparam /etc/nginx/ssl/dhparam.pem;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;

    root /var/www/html;
    index index.html index.htm index.nginx-debian.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```
Активация конфигурации и отключение HTTP по умолчанию
```Bash
sudo ln -s /etc/nginx/sites-available/https-site /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/default
```
Проверка конфигурации и перезапуск NGINX
`sudo nginx -t`
Если вывод: `nginx: configuration file /etc/nginx/nginx.conf test is successful`, тогда перезапустить nginx

### Добавление исключения для самоподписанного сертификата в системе
Поскольку сертификат самоподписанный, браузер будет показывать предупреждение. Нужно добавить его в исключения.
- Откройте браузер (например, Firefox) и перейдите по адресу `https://localhost`.
- Вы увидите предупреждение "Warning: Potential Security Risk Ahead".
- Нажмите **"Advanced..."** → **"Accept the Risk and Continue"**.
- **Важно:** Сделайте это до начала захвата трафика, чтобы браузер не переключался на HTTP.
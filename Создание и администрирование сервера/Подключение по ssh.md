**Secure Shell (SSH)** — это сетевой протокол, который используется для безопасного соединения между клиентом и сервером. Каждое взаимодействие шифруется.
`sudo apt install openssh-server` - установка ssh 

`sudo systemctl status ssh` - проверка работы (в ответ должен выводиться active)
`sudo systemctl enable --now ssh` - запуск в ручную если служб не активна
`sudo ufw allow ssh` - открытие порта для ssh если включен брендмауэр

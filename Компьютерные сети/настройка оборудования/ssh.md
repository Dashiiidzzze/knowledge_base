[[Подключение по ssh]] - про обычный ssh

```
configure terminal
hostname R1          # Задаем имя устройства
ip domain-name lab.local  # Обязательно для генерации ключей
crypto key generate rsa   # Генерация RSA-ключей (минимум 1024 бит)
```
выбрать ключ не менее 700 длиной
```
username admin secret admin123   # Создаем пользователя "admin" с паролем "admin123"
```
**Настройка VTY (виртуальных терминалов) для SSH:**
```
line vty 0 4
 transport input ssh   # Разрешаем только SSH (не telnet)
 login local           # Аутентификация по локальной БД пользователей
 exit
ip ssh version 2      # Включаем SSH версии 2
ip ssh time-out 60    # Таймаут сессии (сек)
ip ssh authentication-retries 3  # Макс. попыток ввода пароля
end
write memory         # Сохраняем конфигурацию
```

**Подключение от клиента:**
```
ssh -l admin 192.168.1.1  # Подключение под пользователем "admin"


exit                      # выход
```

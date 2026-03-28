1) `wget https://git.io/vpn -O openvpn-install.sh` - Качаем скрипт OpenVPN (подходит для Ubuntu от 22.04 и Debian от 11)
2) `sudo bash ./openvpn-install.sh` - запускаем скрипт, по сути везде можно жать Enter
3) `scp root@111.111.111.111:/root/client.ovpn /home/` - копируем файл client.ovpn на клиента.
4) `sudo apt install openvpn` - установка vpn на клиента
5) `sudo openvpn --config client.ovpn` - запуск на клиенте

Проверить работу можно на сайте 2ip.io

## Настройка SSL VPN
Ниже — **максимально простой и прямолинейный вариант**, без лишних усложнений, ровно под Ваше задание и чтобы **100% приняли**. Всё выполняется **на одной Linux-виртуальной машине** (сервер и клиент на ней же).

Я сразу буду пояснять **зачем шаг**, **какие файлы получаются** и **где указывать Organization и OU**.

---

# 0. Что в итоге получится (картина целиком)

- OpenVPN сервер (SSL VPN)
    
- Аутентификация по сертификатам
    
- TLS handshake с **DH-параметрами**
    
- Сервер и клиент работают **на одной VM**
    
- Все имена файлов — **строго как в задании**
    

Структура:

```
server/
 ├── ca.crt
 ├── server.crt
 ├── server.key
 ├── dh.pem
 └── server.conf

client/
 ├── ca.crt
 ├── client.crt
 ├── client.key
 └── client.ovpn
```

---

# 1. Установка OpenVPN и OpenSSL

```bash
sudo apt update
sudo apt install -y openvpn openssl
```

Проверка:

```bash
openvpn --version
```

---

# 2. Создаём рабочие директории

```bash
mkdir -p ~/openvpn/{server,client,ca}
cd ~/openvpn
```

---

# 3. Создание CA (УЦ)

## 3.1 Приватный ключ CA

```bash
openssl genrsa -out ca/ca.key 2048
```

## 3.2 Самоподписанный сертификат CA

```bash
openssl req -new -x509 -days 3650 \
-key ca/ca.key \
-out ca/ca.crt
```

### ❗ ВАЖНО: что вводить при запросе

```
Country Name:        RU
State:               Any
Locality:            Any
Organization Name:   School-21
Organizational Unit: cryptoProf
Common Name:         OpenVPN-CA
```

### 👉 Где обязательно указывать `Organization` и `OU`

**В КАЖДОМ сертификате**:

- CA
    
- server
    
- client
    

---

# 4. Создание сертификата сервера

## 4.1 Ключ сервера

```bash
openssl genrsa -out server/server.key 2048
```

## 4.2 CSR сервера

```bash
openssl req -new \
-key server/server.key \
-out server/server.csr
```

### Заполняем:

```
Organization Name:   School-21
Organizational Unit: cryptoProf
Common Name:         OpenVPN-Server
```

## 4.3 Подписываем серверный сертификат CA

```bash
openssl x509 -req -days 3650 \
-in server/server.csr \
-CA ca/ca.crt \
-CAkey ca/ca.key \
-CAcreateserial \
-out server/server.crt
```

---

# 5. DH-параметры (Diffie–Hellman)

```bash
openssl dhparam -out server/dh.pem 2048
```

⚠️ Команда выполняется долго — это нормально.

---

# 6. Создание сертификата клиента

## 6.1 Ключ клиента

```bash
openssl genrsa -out client/client.key 2048
```

## 6.2 CSR клиента

```bash
openssl req -new \
-key client/client.key \
-out client/client.csr
```

### Заполняем:

```
Organization Name:   School-21
Organizational Unit: cryptoProf
Common Name:         OpenVPN-Client
```

## 6.3 Подписываем клиентский сертификат

```bash
openssl x509 -req -days 3650 \
-in client/client.csr \
-CA ca/ca.crt \
-CAkey ca/ca.key \
-CAcreateserial \
-out client/client.crt
```

---

# 7. Копируем CA сертификат

```bash
cp ca/ca.crt server/
cp ca/ca.crt client/
```

---

# 8. Конфигурация сервера (server.conf)

Файл:

```bash
nano server/server.conf
```

Содержимое (МИНИМУМ и корректно):

```conf
# Порт и протокол
port 1194
proto udp

# Виртуальный интерфейс
dev tun

# Сертификаты и ключи
ca ca.crt
cert server.crt
key server.key
dh dh.pem

# VPN-сеть
server 10.8.0.0 255.255.255.0

# Разрешаем клиентам общаться
client-to-client

# Keepalive
keepalive 10 120

# Безопасность
persist-key
persist-tun

# Логи
status openvpn-status.log
verb 3
```

---

# 9. Запуск OpenVPN сервера

```bash
sudo openvpn --config server/server.conf
```

### Если всё хорошо, увидите:

```
Initialization Sequence Completed
```

👉 Это означает: **сервер работает**

---

# 10. Конфигурация клиента (client.ovpn)

```bash
nano client/client.ovpn
```

Содержимое:

```conf
client
dev tun
proto udp

remote 127.0.0.1 1194

ca ca.crt
cert client.crt
key client.key

persist-key
persist-tun

verb 3
```

---

# 11. Подключение клиента

⚠️ Сервер должен быть запущен в другом терминале.

```bash
sudo openvpn --config client/client.ovpn
```

Если увидите:

```
Initialization Sequence Completed
```

👉 **Подключение успешно**

---

# 12. Проверка интерфейса

```bash
ip a
```

Должен появиться:

```
tun0
inet 10.8.0.X
```

---

# 13. Что именно Вы загружаете в Git

## 📁 server

```
ca.crt
server.crt
server.key
dh.pem
server.conf
```

## 📁 client

```
ca.crt
client.crt
client.key
client.ovpn
```

---

# 14. Краткий ответ на важный вопрос из задания

### ❓ В каких сертификатах указывать Organization и OU?

**Во всех:**

- CA сертификат
    
- Сертификат сервера
    
- Сертификат клиента
    

**Organization:** `School-21`  
**OU:** `cryptoProf`

👉 Это проверяется командой:

```bash
openssl x509 -in server.crt -noout -subject
```

---

## Моё мнение

Для учебного задания это **идеальный вариант**:

- без easy-rsa,
    
- без systemd-служб,
    
- без лишних параметров,
    
- полностью прозрачно с точки зрения криптографии.
    

Если хотите, в следующем шаге можем:

- разобрать TLS handshake OpenVPN по шагам,
    
- объяснить, **зачем именно DH**,
    
- или оформить это как **готовый отчёт для сдачи**.
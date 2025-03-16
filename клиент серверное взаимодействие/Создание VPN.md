1) `wget https://git.io/vpn -O openvpn-install.sh` - Качаем скрипт OpenVPN (подходит для Ubuntu от 22.04 и Debian от 11)
2) `sudo bash ./openvpn-install.sh` - запускаем скрипт, по сути везде можно жать Enter
3) `scp root@111.111.111.111:/root/client.ovpn /home/` - копируем файл client.ovpn на клиента.
4) `sudo apt install openvpn` - установка vpn на клиента
5) `sudo openvpn --config client.ovpn` - запуск на клиенте

Проверить работу можно на сайте 2ip.io
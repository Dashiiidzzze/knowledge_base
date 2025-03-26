**МАС адрес (media access control address)** – уникальный идентификатор, назначенный сетевому адаптеру, применяется в сетях стандартов IEEE 802, в основном Ethernet, Wi-Fi и Bluetooth. Официально он называется «идентификатором типа EUI-48».

Используется для идентификации устройств в локальной сети (LAN), и он не должен повторяться в пределах одной сети.
Имеет длину 48 бит (6 байт).
Общепринятого стандарта на написание адреса нет. Может записываться так:
`00:AB:CD:EF:11:22`   или    `00-AB-CD-EF-11-22`   или   `00ab.cdef.1122`

### Как узнать свой MAC
можно посмотреть через `ip addr show` в Linux: здесь `f0:a6:54:a0:19:23` мой MAC
```
2: wlp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether f0:a6:54:a0:19:23 brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.23/24 brd 192.168.0.255 scope global dynamic noprefixroute wlp1s0
       valid_lft 85665sec preferred_lft 85665sec
    inet6 fe80::655c:27a1:452b:5a66/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```


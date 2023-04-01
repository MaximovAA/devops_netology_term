## Задание

1. Подключитесь к публичному маршрутизатору в интернет. Найдите маршрут к вашему публичному IP.

 ```
telnet route-views.routeviews.org
Username: rviews
show ip route x.x.x.x/32
show bgp x.x.x.x/32
```

```
route-views>show ip route *.*.*.*
Routing entry for *.*.*.*/22, supernet
  Known via "bgp 6447", distance 20, metric 0
  Tag 3303, type external
  Last update from 217.192.89.50 2d04h ago
  Routing Descriptor Blocks:
  * 217.192.89.50, from 217.192.89.50, 2d04h ago
      Route metric is 0, traffic share count is 1
      AS Hops 2
      Route tag 3303
      MPLS label: none
route-views>show bgp *.*.*.*
BGP routing table entry for *.*.*.*/22, version 2784144856
Paths: (21 available, best #21, table default)
  Not advertised to any peer
  Refresh Epoch 1
  3267 35598 49120
  ...
```

2. Создайте dummy-интерфейс в Ubuntu. Добавьте несколько статических маршрутов. Проверьте таблицу маршрутизации.

```
Создаем 2 файла .dev и network для dummy, после systemctl restart systemd-networkd проверяем
amaksimov@git:/etc/systemd/network$ ls
dum.netdev  dum.network
amaksimov@git:/etc/systemd/network$ more dum.netdev
[NetDev]
Name=dum
Kind=dummy
amaksimov@git:/etc/systemd/network$ nmcli
dum: connected (externally) to dum
        "dum"
        dummy, C2:50:AE:7F:E5:9D, sw, mtu 1500
        inet4 172.16.1.25/16
        route4 172.16.0.0/16 metric 0
        inet6 fe80::c050:aeff:fe7f:e59d/64
        route6 fe80::/64 metric 256


amaksimov@git:/etc/systemd/network$ ifconfig -s
Iface      MTU    RX-OK RX-ERR RX-DRP RX-OVR    TX-OK TX-ERR TX-DRP TX-OVR Flg
dum       1500        0      0      0 0            35      0      0      0 BORU
enp0s3    1500   237863      0      0 0         55294      0      0      0 BMRU
lo       65536     7572      0      0 0          7572      0      0      0 LRU

Так как в моем стенде используется Network Manager пример создания маршрута указываю через nmcli
root@git:/etc/NetworkManager/system-connections# sudo nmcli connection modify Wired\ connection\ 1 +ipv4.routes "10.20.30.0/24 *.*.*.*"
root@git:/etc/NetworkManager/system-connections# route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         router          0.0.0.0         UG    100    0        0 enp0s3
10.20.30.0      router          255.255.255.0   UG    100    0        0 enp0s3
link-local      0.0.0.0         255.255.0.0     U     1000   0        0 enp0s3
172.16.0.0      0.0.0.0         255.255.0.0     U     0      0        0 dum
*.*.*.*         0.0.0.0         255.255.255.0   U     100    0        0 enp0s3
router          0.0.0.0         255.255.255.255 UH    100    0        0 enp0s3
root@git:/etc/NetworkManager/system-connections#

```

3. Проверьте открытые TCP-порты в Ubuntu. Какие протоколы и приложения используют эти порты? Приведите несколько примеров.

```
ss -tn

State    Recv-Q    Send-Q       Local Address:Port        Peer Address:Port     Process
ESTAB    0         0            *.*.*.*:22        *.*.*.*:55036

ESTAB    0         0            *.*.*.*:ssh        *.*.*.*:55036
```

4. Проверьте используемые UDP-сокеты в Ubuntu. Какие протоколы и приложения используют эти порты?

```
ss -un
Recv-Q    Send-Q              Local Address:Port         Peer Address:Port      Process
0         0            *.*.*.*%enp0s3:68       *.*.*.*:67

0         0            *.*.*.*%enp0s3:bootpc       *.*.*.*:bootps
```

5. Используя diagrams.net, создайте L3-диаграмму вашей домашней сети или любой другой сети, с которой вы работали. 

*В качестве решения пришлите ответы на вопросы, опишите, как они были получены, и приложите скриншоты при необходимости.*

```

```

 ---
 
## Задание со звёздочкой* 

Это самостоятельное задание, его выполнение необязательно.

6. Установите Nginx, настройте в режиме балансировщика TCP или UDP.

```

```

7. Установите bird2, настройте динамический протокол маршрутизации RIP.

```

```

8. Установите Netbox, создайте несколько IP-префиксов, и, используя curl, проверьте работу API.

```

```

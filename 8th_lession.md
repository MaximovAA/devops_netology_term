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
BGP routing table entry for 193.107.148.0/22, version 2784144856
Paths: (21 available, best #21, table default)
  Not advertised to any peer
  Refresh Epoch 1
  3267 35598 49120

```

2. Создайте dummy-интерфейс в Ubuntu. Добавьте несколько статических маршрутов. Проверьте таблицу маршрутизации.

```

```

3. Проверьте открытые TCP-порты в Ubuntu. Какие протоколы и приложения используют эти порты? Приведите несколько примеров.

```

```

4. Проверьте используемые UDP-сокеты в Ubuntu. Какие протоколы и приложения используют эти порты?

```

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

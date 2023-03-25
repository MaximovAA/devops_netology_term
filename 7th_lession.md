## Задание

1. Проверьте список доступных сетевых интерфейсов на вашем компьютере. Какие команды есть для этого в Linux и в Windows?
```
Привер вывода
Linix:
amaksimov@git:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
   
Windows:
Адаптер Ethernet Ethernet:

   Описание. . . . . . . . . . . . . : Realtek Gaming 2.5GbE Family Controller
   
Адаптер Ethernet Ethernet 2:

   Описание. . . . . . . . . . . . . : VirtualBox Host-Only Ethernet Adapter
   

Адаптер Ethernet Ethernet 3:

  Описание. . . . . . . . . . . . . : VirtualBox Host-Only Ethernet Adapter #2
   
```

2. Какой протокол используется для распознавания соседа по сетевому интерфейсу? Какой пакет и команды есть в Linux для этого?
```
LLDP – протокол для обмена информацией между соседними устройствами,
позволяет определить к какому порту коммутатора подключен сервер.

amaksimov@git:~$ sudo lldpctl
-------------------------------------------------------------------------------
LLDP neighbors:
-------------------------------------------------------------------------------
Interface:    enp0s3, via: LLDP, RID: 1, Time: 0 day, 00:07:24
  Chassis:
    ChassisID:    mac 
    SysName:      
    SysDescr:     
    MgmtIP:       
    MgmtIface:    
    Capability:   
    Capability:   
    Capability:   
  Port:
    PortID:       
    TTL:          
-------------------------------------------------------------------------------
Interface:    enp0s3, via: LLDP, RID: 2, Time: 0 day, 00:06:38
  Chassis:
    ChassisID:    mac 
  Port:
    PortID:       mac 
    TTL:          
    PMD autoneg:  
      Adv:          
      MAU oper type: 
  LLDP-MED:
    Device Type:  
    Capability:   

```

3. Какая технология используется для разделения L2-коммутатора на несколько виртуальных сетей? Какой пакет и команды есть в Linux для этого? Приведите пример конфига.
```
VLAN - виртуальная сеть
На примере конфига из призентации мы создаем интерфейс с автозапуском и статическим адресом с Vlan ID 1400 и указываем ему основным физический интерфейс eth0
auto vlan1400
iface vlan1400 inet static
        address 192.168.1.1
        netmask 255.255.255.0
        vlan_raw_device eth0
```

4. Какие типы агрегации интерфейсов есть в Linux? Какие опции есть для балансировки нагрузки? Приведите пример конфига.
```
LAG в Linux называется – бондинг

auto bond0

В данном примере мы создаем интерфейс bond0  со статикой и указываем ему физические интерфейсы eth0 eth1 в режиме резервирования - active-backup
iface bond0 inet static
    address 10.31.1.5
    netmask 255.255.255.0
    network 10.31.1.0
    gateway 10.31.1.254
    bond-slaves eth0 eth1
    bond-mode active-backup
    bond-miimon 100
    bond-downdelay 200
    bond-updelay 200

Для балансировки возможно использование режимов: 
balance-rr
balance-xor
balance-tlb
balance-alb

Чаще всего используется round robin, так как данный режим работы широко распространен и поддерживается многими устройствами и виртуализацией.
```

5. Сколько IP-адресов в сети с маской /29 ? Сколько /29 подсетей можно получить из сети с маской /24. Приведите несколько примеров /29 подсетей внутри сети 10.10.10.0/24.
```

```

6. Задача: вас попросили организовать стык между двумя организациями. Диапазоны 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 уже заняты. Из какой подсети допустимо взять частные IP-адреса? Маску выберите из расчёта — максимум 40–50 хостов внутри подсети.
```

```

7. Как проверить ARP-таблицу в Linux, Windows? Как очистить ARP-кеш полностью? Как из ARP-таблицы удалить только один нужный IP?
```

```

*В качестве решения отправьте ответы на вопросы и опишите, как они были получены.*

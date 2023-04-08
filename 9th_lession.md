## Задание

1. Установите плагин Bitwarden для браузера. Зарегестрируйтесь и сохраните несколько паролей.
```
Ставим на Firefox
```
![bitward](https://github.com/MaximovAA/devops_netology_term/blob/main/bitward.jpg "Пример")  


2. Установите Google Authenticator на мобильный телефон. Настройте вход в Bitwarden-акаунт через Google Authenticator OTP.
```
Добавил в уже установленный Google Authenticator
```
![2fa](https://github.com/MaximovAA/devops_netology_term/blob/main/2fa.jpg "Пример")
3. Установите apache2, сгенерируйте самоподписанный сертификат, настройте тестовый сайт для работы по HTTPS.
```
root@deburunta:/home/amaksimov# /usr/sbin/apache2ctl configtest
Syntax OK
```
![apache](https://github.com/MaximovAA/devops_netology_term/blob/main/apache.jpg "Пример")
![cert](https://github.com/MaximovAA/devops_netology_term/blob/main/cert.jpg "Пример")  

4. Проверьте на TLS-уязвимости произвольный сайт в интернете (кроме сайтов МВД, ФСБ, МинОбр, НацБанк, РосКосмос, РосАтом, РосНАНО и любых госкомпаний, объектов КИИ, ВПК и т. п.).
```
git clone --depth 1 https://github.com/drwetter/testssl.sh.git
cd testssl.sh
./testssl.sh -U --sneaky https://test-test.su/ проверяем сайт возданный в пункте 3
```
![vulners](https://github.com/MaximovAA/devops_netology_term/blob/main/vulners.jpg "Пример")  

5. Установите на Ubuntu SSH-сервер, сгенерируйте новый приватный ключ. Скопируйте свой публичный ключ на другой сервер. Подключитесь к серверу по SSH-ключу.
```
Создаем связку ключей
ssh-keygen
ssh-copy-id -i amaks.pub amaksimov@*
amaksimov@deburunta:~/.ssh$ ssh -i amaks amaksimov@*

amaksimov@git:~$
```
 
6. Переименуйте файлы ключей из задания 5. Настройте файл конфигурации SSH-клиента так, чтобы вход на удалённый сервер осуществлялся по имени сервера.
```
Переименовываем ключи через mv, создаем файл конфига в котором указываем параметры подключения (пример ниже). Подключаемся без каких-либо ошибок.

touch ~/.ssh/config && chmod 600 ~/.ssh/config
vim config

Host my_server
HostName 192.168.1.100
IdentityFile ~/.ssh/some_server.key
User my_user
```

7. Соберите дамп трафика утилитой tcpdump в формате pcap, 100 пакетов. Откройте файл pcap в Wireshark.
```
Ставим утилиту apt install tcpdump
Собираем дамп файл tcpdump -c 100 -w 0001.pcap
Далее я выдернул 0001.pcap средствами winscp и открыл в виндовой версии wireshark
```
![pcap](https://github.com/MaximovAA/devops_netology_term/blob/main/pcap.jpg "Пример некоторых выводов")

8. Просканируйте хост scanme.nmap.org. Какие сервисы запущены?
```
В выводе в таблице можно увидеть стоблец Service

root@deburunta:/home/amaksimov# nmap -O scanme.nmap.org
Starting Nmap 7.80 ( https://nmap.org ) at 2023-04-08 20:35 MSK
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.18s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
Not shown: 993 closed ports
PORT      STATE    SERVICE
22/tcp    open     ssh
80/tcp    open     http
135/tcp   filtered msrpc
139/tcp   filtered netbios-ssn
445/tcp   filtered microsoft-ds
9929/tcp  open     nping-echo
31337/tcp open     Elite
Aggressive OS guesses: Linux 2.6.32 - 3.13 (93%), Linux 2.6.22 - 2.6.36 (91%), Linux 3.10 - 4.11 (91%), Linux 3.10 (91%), Linux 2.6.32 (90%), Linux 3.2 - 4.9 (90%), Linux 2.6.32 - 3.10 (90%), Linux 2.6.18 (90%), Linux 3.16 - 4.6 (90%), HP P2000 G3 NAS device (89%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 23 hops

OS detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 14.06 seconds

```

9. Установите и настройте фаервол UFW на веб-сервер из задания 3. Откройте доступ снаружи только к портам 22, 80, 443.
```
amaksimov@deburunta:~$ sudo ufw enable
Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
Firewall is active and enabled on system startup
amaksimov@deburunta:~$ sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
80/tcp                     ALLOW       Anywhere
22/tcp                     ALLOW       Anywhere
443/tcp                    ALLOW       Anywhere
80/tcp (v6)                ALLOW       Anywhere (v6)
22/tcp (v6)                ALLOW       Anywhere (v6)
443/tcp (v6)               ALLOW       Anywhere (v6)


```

------

## Задание

**Шаг 1.** Работа c HTTP через telnet.

- Подключитесь утилитой telnet к сайту stackoverflow.com:

`telnet stackoverflow.com 80`
 
- Отправьте HTTP-запрос:

```bash
GET /questions HTTP/1.0
HOST: stackoverflow.com
[press enter]
[press enter]
```
*В ответе укажите полученный HTTP-код и поясните, что он означает.*

```
vagrant@sysadm-fs:~$ telnet stackoverflow.com 80
Trying 151.101.65.69...
Connected to stackoverflow.com.
Escape character is '^]'.
GET /questions HTTP/1.0
HOST: stackoverflow.com

HTTP/1.1 403 Forbidden
Connection: close
Content-Length: 1926
Server: Varnish
Retry-After: 0
Content-Type: text/html
Accept-Ranges: bytes
Date: Thu, 16 Mar 2023 18:56:35 GMT
Via: 1.1 varnish
X-Served-By: cache-bma1669-BMA
X-Cache: MISS
X-Cache-Hits: 0
X-Timer: S1678992995.411273,VS0,VE1
X-DNS-Prefetch-Control: off

Ошибка 403 (Forbidden/Доступ запрещён) возвращается клиенту сервером, когда доступ к указанному ресурсу ограничен.
```

**Шаг 2.** Повторите задание 1 в браузере, используя консоль разработчика F12:

 - откройте вкладку `Network`;
 - отправьте запрос [http://stackoverflow.com](http://stackoverflow.com);
 - найдите первый ответ HTTP-сервера, откройте вкладку `Headers`;
 - укажите в ответе полученный HTTP-код;
 - проверьте время загрузки страницы и определите, какой запрос обрабатывался дольше всего;
 - приложите скриншот консоли браузера в ответ.

```
Состояние
200
OK

```
![stackoverflow](https://github.com/MaximovAA/devops_netology_term/blob/main/stackoverflow.jpg "Пример вывода команд")

**Шаг 3.** Какой IP-адрес у вас в интернете?

![whois](https://github.com/MaximovAA/devops_netology_term/blob/main/whois.jpg "Пример вывода команд")

```
dig TXT +short o-o.myaddr.l.google.com @ns1.google.com

```

**Шаг 4.** Какому провайдеру принадлежит ваш IP-адрес? Какой автономной системе AS? Воспользуйтесь утилитой `whois`.

```
Выполняем в консоли whois нашipадрес
смотрим вывод в полях
descr:
origin:
```

**Шаг 5.** Через какие сети проходит пакет, отправленный с вашего компьютера на адрес 8.8.8.8? Через какие AS? Воспользуйтесь утилитой `traceroute`.

```
С 4го хопа

 4  178.18.227.7 [AS50952]  6.107 ms  6.029 ms  6.088 ms
 5  108.170.250.99 [AS15169]  6.708 ms * 108.170.250.146 [AS15169]  11.751 ms
 6  216.239.51.32 [AS15169]  21.507 ms 142.251.238.82 [AS15169]  62.696 ms 142.250.238.138 [AS15169]  22.256 ms
 7  216.239.48.224 [AS15169]  39.394 ms 142.251.238.72 [AS15169]  23.480 ms 72.14.232.190 [AS15169]  23.181 ms
 8  142.250.56.131 [AS15169]  23.135 ms 142.250.236.77 [AS15169]  25.046 ms 216.239.47.165 [AS15169]  24.744 ms
 9  * * *
10  * * *
11  * * *
12  * * *
13  * * *
14  * * *
15  * * *
16  * * *
17  * * *
18  8.8.8.8 [AS15169/AS263411]  22.742 ms  24.191 ms  24.185 ms
amaksimov@git:~$


```

**Шаг 6.** Повторите задание 5 в утилите `mtr`. На каком участке наибольшая задержка — delay?

![mtr](https://github.com/MaximovAA/devops_netology_term/blob/main/mtr.jpg "Пример вывода команд")

```
Так же с 4го хопа
наибольшая средняя задержка была на хопе 3, 22,9

```

**Шаг 7.** Какие DNS-сервера отвечают за доменное имя dns.google? Какие A-записи? Воспользуйтесь утилитой `dig`.

```
amaksimov@git:~$ dig dns.google

; <<>> DiG 9.18.1-1ubuntu1.3-Ubuntu <<>> dns.google
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 55447
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 13, ADDITIONAL: 10

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
;; QUESTION SECTION:
;dns.google.                    IN      A

;; ANSWER SECTION:
dns.google.             524     IN      A       8.8.8.8
dns.google.             524     IN      A       8.8.4.4

;; AUTHORITY SECTION:
.                       18723   IN      NS      b.root-servers.net.
.                       18723   IN      NS      g.root-servers.net.
.                       18723   IN      NS      k.root-servers.net.
.                       18723   IN      NS      d.root-servers.net.
.                       18723   IN      NS      a.root-servers.net.
.                       18723   IN      NS      f.root-servers.net.
.                       18723   IN      NS      e.root-servers.net.
.                       18723   IN      NS      m.root-servers.net.
.                       18723   IN      NS      j.root-servers.net.
.                       18723   IN      NS      h.root-servers.net.
.                       18723   IN      NS      c.root-servers.net.
.                       18723   IN      NS      l.root-servers.net.
.                       18723   IN      NS      i.root-servers.net.

;; ADDITIONAL SECTION:
b.root-servers.net.     29810   IN      A       199.9.14.201
g.root-servers.net.     85951   IN      A       192.112.36.4
d.root-servers.net.     85947   IN      A       199.7.91.13
a.root-servers.net.     17523   IN      A       198.41.0.4
e.root-servers.net.     51884   IN      A       192.203.230.10
m.root-servers.net.     19753   IN      A       202.12.27.33
h.root-servers.net.     55539   IN      A       198.97.190.53
l.root-servers.net.     85874   IN      A       199.7.83.42
i.root-servers.net.     85929   IN      A       192.36.148.17

;; Query time: 7 msec
;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
;; WHEN: Thu Mar 16 22:47:57 MSK 2023
;; MSG SIZE  rcvd: 426


```

**Шаг 8.** Проверьте PTR записи для IP-адресов из задания 7. Какое доменное имя привязано к IP? Воспользуйтесь утилитой `dig`.

*В качестве ответов на вопросы приложите лог выполнения команд в консоли или скриншот полученных результатов.*

```
amaksimov@git:~$ dig -x 8.8.4.4

; <<>> DiG 9.18.1-1ubuntu1.3-Ubuntu <<>> -x 8.8.4.4
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 27856
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
;; QUESTION SECTION:
;4.4.8.8.in-addr.arpa.          IN      PTR

;; ANSWER SECTION:
4.4.8.8.in-addr.arpa.   86400   IN      PTR     dns.google.

;; Query time: 4131 msec
;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
;; WHEN: Thu Mar 16 22:54:51 MSK 2023
;; MSG SIZE  rcvd: 73

amaksimov@git:~$ dig -x 8.8.8.8

; <<>> DiG 9.18.1-1ubuntu1.3-Ubuntu <<>> -x 8.8.8.8
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 43540
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
;; QUESTION SECTION:
;8.8.8.8.in-addr.arpa.          IN      PTR

;; ANSWER SECTION:
8.8.8.8.in-addr.arpa.   7162    IN      PTR     dns.google.

;; Query time: 0 msec
;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
;; WHEN: Thu Mar 16 22:55:09 MSK 2023
;; MSG SIZE  rcvd: 73

amaksimov@git:~$


```

----

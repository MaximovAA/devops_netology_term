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


```

**Шаг 7.** Какие DNS-сервера отвечают за доменное имя dns.google? Какие A-записи? Воспользуйтесь утилитой `dig`.

```


```

**Шаг 8.** Проверьте PTR записи для IP-адресов из задания 7. Какое доменное имя привязано к IP? Воспользуйтесь утилитой `dig`.

*В качестве ответов на вопросы приложите лог выполнения команд в консоли или скриншот полученных результатов.*

```


```

----

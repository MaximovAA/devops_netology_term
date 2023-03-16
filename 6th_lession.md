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


```

**Шаг 6.** Повторите задание 5 в утилите `mtr`. На каком участке наибольшая задержка — delay?

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
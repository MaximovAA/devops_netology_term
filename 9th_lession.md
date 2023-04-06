## Задание

1. Установите плагин Bitwarden для браузера. Зарегестрируйтесь и сохраните несколько паролей.
```

```
![bitward](https://github.com/MaximovAA/devops_netology_term/blob/main/bitward.jpg "Пример")  


2. Установите Google Authenticator на мобильный телефон. Настройте вход в Bitwarden-акаунт через Google Authenticator OTP.
```

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

```

5. Установите на Ubuntu SSH-сервер, сгенерируйте новый приватный ключ. Скопируйте свой публичный ключ на другой сервер. Подключитесь к серверу по SSH-ключу.
```

```
 
6. Переименуйте файлы ключей из задания 5. Настройте файл конфигурации SSH-клиента так, чтобы вход на удалённый сервер осуществлялся по имени сервера.
```

```

7. Соберите дамп трафика утилитой tcpdump в формате pcap, 100 пакетов. Откройте файл pcap в Wireshark.
```

```

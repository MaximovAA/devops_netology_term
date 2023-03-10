------

## Задание

1. На лекции вы познакомились с [node_exporter](https://github.com/prometheus/node_exporter/releases). В демонстрации его исполняемый файл запускался в background. Этого достаточно для демо, но не для настоящей production-системы, где процессы должны находиться под внешним управлением. Используя знания из лекции по systemd, создайте самостоятельно простой [unit-файл](https://www.freedesktop.org/software/systemd/man/systemd.service.html) для node_exporter:

    * поместите его в автозагрузку;
    * предусмотрите возможность добавления опций к запускаемому процессу через внешний файл (посмотрите, например, на `systemctl cat cron`);
    * удостоверьтесь, что с помощью systemctl процесс корректно стартует, завершается, а после перезагрузки автоматически поднимается.

```
Конфиг службы с предусмотренным внешним файлом:
[Unit]
Description=NodeExporter

[Service]
EnvironmentFile=-/etc/default/node_export_conf
TimeoutStartSec=0
User=nodeusr
ExecStart=/usr/local/bin/node_exporter \
--web.listen-address=:9100

[Install]
WantedBy=multi-user.target

Автозапуск:
systemctl enable node_exporter

```
![nodeexp](https://github.com/MaximovAA/devops_netology_term/blob/main/nodeexp.jpg "Пример вывода команд")

2. Изучите опции node_exporter и вывод `/metrics` по умолчанию. Приведите несколько опций, которые вы бы выбрали для базового мониторинга хоста по CPU, памяти, диску и сети.
```
В минимальной реализации я бы выбрал:
{collector="cpufreq"}
{collector="meminfo"}
{collector="diskstats"}
{collector="netstat"}
```
3. Установите в свою виртуальную машину [Netdata](https://github.com/netdata/netdata). Воспользуйтесь [готовыми пакетами](https://packagecloud.io/netdata/netdata/install) для установки (`sudo apt install -y netdata`). 
   
   После успешной установки:
   
    * в конфигурационном файле `/etc/netdata/netdata.conf` в секции [web] замените значение с localhost на `bind to = 0.0.0.0`;
    * добавьте в Vagrantfile проброс порта Netdata на свой локальный компьютер и сделайте `vagrant reload`:

    ```bash
    config.vm.network "forwarded_port", guest: 19999, host: 19999
    ```

    После успешной перезагрузки в браузере на своём ПК (не в виртуальной машине) вы должны суметь зайти на `localhost:19999`. Ознакомьтесь с метриками, которые по умолчанию собираются Netdata, и с комментариями, которые даны к этим метрикам.

```
На скриншоте ниже выделил метрики и комментарии к ним.
```
![Netdata](https://github.com/MaximovAA/devops_netology_term/blob/main/Netdata.jpg "Пример вывода команд")

4. Можно ли по выводу `dmesg` понять, осознаёт ли ОС, что загружена не на настоящем оборудовании, а на системе виртуализации?
```
Достаточно много упоминаний в выводе к примеру устройств, но самое емкое на мой взгляд
vboxguest: host-version: 7.0.6r155176 0x8000000f
```
5. Как настроен sysctl `fs.nr_open` на системе по умолчанию? Определите, что означает этот параметр. Какой другой существующий лимит не позволит достичь такого числа (`ulimit --help`)?
```
root@git:/home/amaksimov# cat /proc/sys/fs/nr_open
1048576
root@git:/home/amaksimov# /sbin/sysctl -n fs.nr_open
1048576
root@git:/home/amaksimov# ulimit -n
1024

If LIMIT is given, it is the new value of the specified resource; the
    special LIMIT values `soft', `hard', and `unlimited' stand for the
    current soft limit, the current hard limit, and no limit, respectively.
    Otherwise, the current value of the specified resource is printed.  If
    no option is given, then -f is assumed.

    Values are in 1024-byte increments, except for -t, which is in seconds,
    -p, which is in increments of 512 bytes, and -u, which is an unscaled
    number of processes.


```
6. Запустите любой долгоживущий процесс (не `ls`, который отработает мгновенно, а, например, `sleep 1h`) в отдельном неймспейсе процессов; покажите, что ваш процесс работает под PID 1 через `nsenter`. Для простоты работайте в этом задании под root (`sudo -i`). Под обычным пользователем требуются дополнительные опции (`--map-root-user`) и т. д.
```
unshare -f -p --mount-proc sleep 1h
ps aux
```
![sleep1h](https://github.com/MaximovAA/devops_netology_term/blob/main/sleep1h.jpg "Пример вывода команд")

7. Найдите информацию о том, что такое `:(){ :|:& };:`. Запустите эту команду в своей виртуальной машине Vagrant с Ubuntu 20.04 (**это важно, поведение в других ОС не проверялось**). Некоторое время всё будет плохо, после чего (спустя минуты) — ОС должна стабилизироваться. Вызов `dmesg` расскажет, какой механизм помог автоматической стабилизации.  
Как настроен этот механизм по умолчанию, и как изменить число процессов, которое можно создать в сессии?

*В качестве решения отправьте ответы на вопросы и опишите, как эти ответы были получены.*кйй
```
В теории должен был отработать механизм OOM Killer, но за более чем 2 часа так и не справился на Ubuntu 22.04.1 .
OOM Killer — защитный механизм ядра Linux, призванный решать проблемы с нехваткой памяти. При исчерпании доступной памяти он принудительно «убивает» наиболее подходящий по приоритетам процесс, отправляя ему сигнал KILL.
```
----

### Правила приёма домашнего задания

В личном кабинете отправлена ссылка на .md-файл в вашем репозитории.

-----

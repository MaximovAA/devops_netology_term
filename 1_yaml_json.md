------

## Задание 1

Мы выгрузили JSON, который получили через API-запрос к нашему сервису:

```
    { "info" : "Sample JSON output from our service\t",
        "elements" :[
            { "name" : "first",
            "type" : "server",
            "ip" : 7175 
            }
            { "name" : "second",
            "type" : "proxy",
            "ip : 71.78.22.43
            }
        ]
    }
```
  Нужно найти и исправить все ошибки, которые допускает наш сервис.

### Ваш скрипт:

```
{ "info" : "Sample JSON output from our service\t",
        "elements" :[
            {"name" : "first",
            "type" : "server",
            "ip" : 7175
            },
            {"name" : "second",
            "type" : "proxy",
            "ip" : "71.78.22.43"
            }
        ]
}


```

---

## Задание 2

В прошлый рабочий день мы создавали скрипт, позволяющий опрашивать веб-сервисы и получать их IP. К уже реализованному функционалу нам нужно добавить возможность записи JSON и YAML-файлов, описывающих наши сервисы. 

Формат записи JSON по одному сервису: `{ "имя сервиса" : "его IP"}`. 

Формат записи YAML по одному сервису: `- имя сервиса: его IP`. 

Если в момент исполнения скрипта меняется IP у сервиса — он должен так же поменяться в YAML и JSON-файле.

### Ваш скрипт:

```python
#!/usr/bin/env python3

import socket, time
import json, yaml

hostlist = {"drive.google.com":None, "mail.google.com":None, "google.com":None}

while 1==1:
    for i in hostlist:
        dns = i
        answer = socket.gethostbyname(i)
        time.sleep(1)
        print (hostlist)
        data = [{dns:answer}]
        if answer != hostlist[i]:
            print (dns, 'ERROR',answer,'IP mismath',hostlist[i])
            hostlist[i] = answer
        with open(dns+'.yml', 'w') as ym:
            ym.write(yaml.dump(data, explicit_start=True, explicit_end=True))
        with open(dns+'.json', 'w') as js:
            jsdata = json.dumps(data)
            js.write(jsdata)


```

### Вывод скрипта при запуске во время тестирования:

```
amaksimov@deburunta:~$ ./4.py
{'drive.google.com': None, 'mail.google.com': None, 'google.com': None}
drive.google.com ERROR 64.233.165.194 IP mismath None
{'drive.google.com': '64.233.165.194', 'mail.google.com': None, 'google.com': None}
mail.google.com ERROR 173.194.222.83 IP mismath None
{'drive.google.com': '64.233.165.194', 'mail.google.com': '173.194.222.83', 'google.com': None}
google.com ERROR 173.194.73.100 IP mismath None
{'drive.google.com': '64.233.165.194', 'mail.google.com': '173.194.222.83', 'google.com': '173.194.73.100'}
{'drive.google.com': '64.233.165.194', 'mail.google.com': '173.194.222.83', 'google.com': '173.194.73.100'}
mail.google.com ERROR 173.194.222.17 IP mismath 173.194.222.83
{'drive.google.com': '64.233.165.194', 'mail.google.com': '173.194.222.17', 'google.com': '173.194.73.100'}
google.com ERROR 173.194.73.113 IP mismath 173.194.73.100
{'drive.google.com': '64.233.165.194', 'mail.google.com': '173.194.222.17', 'google.com': '173.194.73.113'}
{'drive.google.com': '64.233.165.194', 'mail.google.com': '173.194.222.17', 'google.com': '173.194.73.113'}
mail.google.com ERROR 173.194.222.19 IP mismath 173.194.222.17
{'drive.google.com': '64.233.165.194', 'mail.google.com': '173.194.222.19', 'google.com': '173.194.73.113'}
google.com ERROR 173.194.73.101 IP mismath 173.194.73.113
{'drive.google.com': '64.233.165.194', 'mail.google.com': '173.194.222.19', 'google.com': '173.194.73.101'}
{'drive.google.com': '64.233.165.194', 'mail.google.com': '173.194.222.19', 'google.com': '173.194.73.101'}
mail.google.com ERROR 173.194.222.18 IP mismath 173.194.222.19
{'drive.google.com': '64.233.165.194', 'mail.google.com': '173.194.222.18', 'google.com': '173.194.73.101'}
google.com ERROR 173.194.73.138 IP mismath 173.194.73.101
{'drive.google.com': '64.233.165.194', 'mail.google.com': '173.194.222.18', 'google.com': '173.194.73.138'}
^CTraceback (most recent call last):
  File "/home/amaksimov/./4.py", line 12, in <module>
    time.sleep(1)
KeyboardInterrupt

```

### JSON-файл(ы), который(е) записал ваш скрипт:

```json
amaksimov@deburunta:~$ cat drive.google.com.json
[{"drive.google.com": "64.233.165.194"}]
amaksimov@deburunta:~$ cat mail.google.com.json
[{"mail.google.com": "173.194.222.18"}]
amaksimov@deburunta:~$ cat google.com.json
[{"google.com": "173.194.73.138"}]

```

### YAML-файл(ы), который(е) записал ваш скрипт:

```yaml
---
amaksimov@deburunta:~$ cat drive.google.com.yml
---
- drive.google.com: 64.233.165.194
...
amaksimov@deburunta:~$ cat google.com.yml
---
- google.com: 173.194.73.138
...
amaksimov@deburunta:~$ cat mail.google.com.yml
---
- mail.google.com: 173.194.222.18
...

```

---

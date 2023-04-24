------

## Задание 1

Есть скрипт:

```python
#!/usr/bin/env python3
a = 1
b = '2'
c = a + b
```

### Вопросы:

| Вопрос  | Ответ |
| ------------- | ------------- |
| Какое значение будет присвоено переменной `c`?  | TypeError: unsupported operand type(s) for +: 'int' and 'str'  |
| Как получить для переменной `c` значение 12?  | c = str(a) + b  |
| Как получить для переменной `c` значение 3?  | c = a + int(b)  |

------

## Задание 2

Мы устроились на работу в компанию, где раньше уже был DevOps-инженер. Он написал скрипт, позволяющий узнать, какие файлы модифицированы в репозитории относительно локальных изменений. Этим скриптом недовольно начальство, потому что в его выводе есть не все изменённые файлы, а также непонятен полный путь к директории, где они находятся. 

Как можно доработать скрипт ниже, чтобы он исполнял требования вашего руководителя?

```python
#!/usr/bin/env python3

import os

bash_command = ["cd ~/netology/sysadm-homeworks", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
        break
```

### Ваш скрипт:

```python
#!/usr/bin/env python3

import os

bash_command = [("cd ~/git-net"), "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
print ('GIT STATUS:', result_os)
is_change = False
#print (is_change)
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print ('Modified: ', prepare_result)
    elif result.find('new file') != -1:
         prepare_result = result.replace('\tnew file:   ', '')
         print('New files: ', prepare_result)


```

### Вывод скрипта при запуске во время тестирования:

```
В рамках скрипта добавил некоторые дополнительные выводы, для наглядности.
```

```
amaksimov@deburunta:~/git-net$ ./1.py
GIT STATUS: On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   1.py
        new file:   123.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   1.py
        modified:   readme.md


New files:  1.py
New files:  123.md
Modified:  1.py
Modified:  readme.md
amaksimov@deburunta:~/git-net$
```

------

## Задание 3

Доработать скрипт выше так, чтобы он не только мог проверять локальный репозиторий в текущей директории, но и умел воспринимать путь к репозиторию, который мы передаём, как входной параметр. Мы точно знаем, что начальство будет проверять работу этого скрипта в директориях, которые не являются локальными репозиториями.

### Ваш скрипт:

```python
#!/usr/bin/env python3

import os
import sys
path = sys.argv[1]
print (path)
bash_command = [("cd " + path), "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
print ('GIT STATUS:', result_os)
is_change = False
#print (is_change)
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print ('Modified in ', path, ': ', prepare_result)
    elif result.find('new file') != -1:
         prepare_result = result.replace('\tnew file:   ', '')
         print('New files in', path, ': ', prepare_result)

```

### Вывод скрипта при запуске во время тестирования:

```
В рамках скрипта добавил некоторые дополнительные выводы, для наглядности.
```

```
amaksimov@deburunta:~/git-net$ ./1.py /home/amaksimov/git-net/
/home/amaksimov/git-net/
GIT STATUS: On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   1.py
        new file:   123.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   1.py
        modified:   readme.md


New files in /home/amaksimov/git-net/ :  1.py
New files in /home/amaksimov/git-net/ :  123.md
Modified in  /home/amaksimov/git-net/ :  1.py
Modified in  /home/amaksimov/git-net/ :  readme.md
amaksimov@deburunta:~/git-net$

```

------

## Задание 4

Наша команда разрабатывает несколько веб-сервисов, доступных по HTTPS. Мы точно знаем, что на их стенде нет никакой балансировки, кластеризации, за DNS прячется конкретный IP сервера, где установлен сервис. 

Проблема в том, что отдел, занимающийся нашей инфраструктурой, очень часто меняет нам сервера, поэтому IP меняются примерно раз в неделю, при этом сервисы сохраняют за собой DNS-имена. Это бы совсем никого не беспокоило, если бы несколько раз сервера не уезжали в такой сегмент сети нашей компании, который недоступен для разработчиков. 

Мы хотим написать скрипт, который: 

- опрашивает веб-сервисы; 
- получает их IP; 
- выводит информацию в стандартный вывод в виде: <URL сервиса> - <его IP>. 

Также должна быть реализована возможность проверки текущего IP сервиса c его IP из предыдущей проверки. Если проверка будет провалена — оповестить об этом в стандартный вывод сообщением: [ERROR] <URL сервиса> IP mismatch: <старый IP> <Новый IP>. Будем считать, что наша разработка реализовала сервисы: `drive.google.com`, `mail.google.com`, `google.com`.

### Ваш скрипт:

```python
#!/usr/bin/env python3
import socket
import time

name = '0'
iplist = [None, None, None]
hosts = ["drive.google.com", "mail.google.com", "google.com"]

while 1 == 1:
    for i in range (0, len(hosts)):
        time.sleep(1)
        ip = socket.gethostbyname(hosts[i])
        name = hosts[i]
        if iplist[i] is None:
            iplist[i] = ip
            print (name, ip)
        elif iplist[i] != ip:
            print ('Новый адрес ',name,' - ',ip,' Старый адрес -',iplist[i])
            iplist[i] = ip

```

### Вывод скрипта при запуске во время тестирования:

```
В моем примере адресация drive.google.com не менялась, а mail.google.com и google.com отвечали с разных адресов. Скрипт записывает в массив последний адрес ответа и указывает его вместе с новым ip адресом.
```
![dns](https://github.com/MaximovAA/devops_netology_term/blob/main/dns.jpg "Пример")
------

1. [Полезные ссылки для модуля «Скриптовые языки и языки разметки».](https://github.com/netology-code/sysadm-homeworks/tree/devsys10/04-script-03-yaml/additional-info)

------

## Задание 1

Есть скрипт:

```bash
a=1
b=2
c=a+b
d=$a+$b
e=$(($a+$b))
```

Какие значения переменным c, d, e будут присвоены? Почему?

| Переменная  | Значение | Обоснование |
| ------------- | ------------- | ------------- |
| `c`  | ???  | ??? |
| `d`  | ???  | ??? |
| `e`  | ???  | ??? |


```
c=a+b					- В данном случае обазначено явное присвоения значения "a+b"
amaksimov@deburunta:~$ echo $c  
a+b
d=$a+$b					- так как переменная d не обявлена численной в нее запишется строковое значение
amaksimov@deburunta:~$ echo $d
1+2
e=$(($a+$b))				- так как успользуются (( )) e не требуется объявлять числовой, в нее запишется сумма a и b
amaksimov@deburunta:~$ echo $e
3
```
----

## Задание 2

На нашем локальном сервере упал сервис, и мы написали скрипт, который постоянно проверяет его доступность, записывая дату проверок до тех пор, пока сервис не станет доступным. После чего скрипт должен завершиться. 

В скрипте допущена ошибка, из-за которой выполнение не может завершиться, при этом место на жёстком диске постоянно уменьшается. Что необходимо сделать, чтобы его исправить:

```bash
while ((1==1)
do
	curl https://localhost:4757
	if (($? != 0))
	then
		date >> curl.log
	fi
done
```

### Ваш скрипт:


```bash
#!/bin/bash
while ((1==1))
do
        curl https://localhost:4757
        echo "значение равно $?"
        if (($? != 0))
        then
                date >> curl.log
        else
        break
        fi
done

amaksimov@deburunta:~$ ./1.sh
curl: (7) Failed to connect to localhost port 4757: Connection refused
значение равно 7



```

---

## Задание 3

Необходимо написать скрипт, который проверяет доступность трёх IP: `192.168.0.1`, `173.194.222.113`, `87.250.250.242` по `80` порту и записывает результат в файл `log`. Проверять доступность необходимо пять раз для каждого узла.

### Ваш скрипт:

```bash
#!/bin/bash
counter=0
while ((1==1))
do
        echo $counter
        curl -Is http://192.168.0.1:80
        if (($? != 0))
        then
        echo "192.168.0.1 не доступен" >> error.log
        else
        echo "192.168.0.1 доступен" >> curl.log
                date >> curl.log
        fi
        curl -Is http://173.194.222.113:80
        if (($? != 0))
        then
        echo "173.194.222.113 не доступен" >> error.log
        else
        echo "173.194.222.113 доступен" >> curl.log
                date >> curl.log
        fi
        curl -Is http://87.250.250.242:80
        if (($? != 0))
        then
        echo "87.250.250.242 не доступен" >> error.log
        else
        echo "87.250.250.242 доступен" >> curl.log
                date >> curl.log
        fi
        if ((counter>=4))
        then
        break
        fi
        counter=$(($counter+1))
done

```
![vulners](https://github.com/MaximovAA/devops_netology_term/blob/main/log.jpg "Пример")


---
## Задание 4

Необходимо дописать скрипт из предыдущего задания так, чтобы он выполнялся до тех пор, пока один из узлов не окажется недоступным. Если любой из узлов недоступен — IP этого узла пишется в файл error, скрипт прерывается.

### Ваш скрипт:

```bash
#!/bin/bash
counter=0
while ((1==1))
do
        echo $counter
        curl -Is http://192.168.0.1:80
        if (($? != 0))
        then
        echo "192.168.0.1 не доступен" >> error.log
        break
        else
        echo "192.168.0.1 доступен" >> curl.log
                date >> curl.log
        fi
        echo $counter
        curl -Is http://173.194.222.113:80
        if (($? != 0))
        then
        echo "173.194.222.113 не доступен" >> error.log
        break
        else
        echo "173.194.222.113 доступен" >> curl.log
                date >> curl.log
        fi
        echo $counter
        curl -Is http://87.250.250.242:80
        if (($? != 0))
        then
        echo "87.250.250.242 не доступен" >> error.log
        break
        else
        echo "87.250.250.242 доступен" >> curl.log
                date >> curl.log
        fi
        counter=$(($counter+1))
done

```
```
Так как в моем случае недоступен первый из проверяемых IP скрипт отрабатывает сразу break по его условию.
amaksimov@deburunta:~$ cat error.log

192.168.0.1 не доступен
amaksimov@deburunta:~$ cat curl.log

amaksimov@deburunta:~$

Если поменять на доступный ip выполняется по кругу с подсчетом итераций.
```



---


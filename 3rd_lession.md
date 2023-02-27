------

## Задание

1. Какой системный вызов делает команда `cd`? 

    В прошлом ДЗ вы выяснили, что `cd` не является самостоятельной  программой. Это `shell builtin`, поэтому запустить `strace` непосредственно на `cd` не получится. Вы можете запустить `strace` на `/bin/bash -c 'cd /tmp'`. В этом случае увидите полный список системных вызовов, которые делает сам `bash` при старте. 

    Вам нужно найти тот единственный, который относится именно к `cd`. Обратите внимание, что `strace` выдаёт результат своей работы в поток stderr, а не в stdout.  
    ```
    newfstatat(AT_FDCWD, "/tmp", {st_mode=S_IFDIR|S_ISVTX|0777, st_size=4096, ...}, 0) = 0
    chdir("/tmp")                           = 0
    ```

2. Попробуйте использовать команду `file` на объекты разных типов в файловой системе. Например:

    ```bash
    vagrant@netology1:~$ file /dev/tty
    /dev/tty: character special (5/0)
    vagrant@netology1:~$ file /dev/sda
    /dev/sda: block special (8/0)
    vagrant@netology1:~$ file /bin/bash
    /bin/bash: ELF 64-bit LSB shared object, x86-64
    ```
    
    Используя `strace`, выясните, где находится база данных `file`, на основании которой она делает свои догадки.  
    ```
    amaksimov@git:~$ file /dev/tty
    /dev/tty: character special (5/0)
    amaksimov@git:~$ file /dev/sda
    /dev/sda: block special (8/0)
    amaksimov@git:~$ file /bin/bash
    /bin/bash: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=33a5554034feb2af38e8c75872058883b2988bc5, for GNU/Linux 3.2.0, stripped
    
    amaksimov@git:~$ strace file
    execve("/usr/bin/file", ["file"], 0x7ffff2610c40 /* 35 vars */) = 0
    ________
    openat(AT_FDCWD, "/usr/share/misc/magic.mgc", O_RDONLY) = 3
    
    Выдержка из man magic в моей ОС:
    This manual page documents the format of magic files as used by the file(1) command, version 5.41.
     The file(1) command identifies the type of a file using, among other tests, a test for whether the
     file contains certain “magic patterns”.  The database of these “magic patterns” is usually located in
     a binary file in /usr/share/misc/magic.mgc or a directory of source text magic pattern fragment files
     in /usr/share/misc/magic.  The database specifies what patterns are to be tested for, what message or
     MIME type to print if a particular pattern is found, and additional information to extract from the
     file.

    ```

3. Предположим, приложение пишет лог в текстовый файл. Этот файл оказался удалён (deleted в lsof), но сказать сигналом приложению переоткрыть файлы или просто перезапустить приложение возможности нет. Так как приложение продолжает писать в удалённый файл, место на диске постепенно заканчивается. Основываясь на знаниях о перенаправлении потоков, предложите способ обнуления открытого удалённого файла, чтобы освободить место на файловой системе.  
```


Перенаправить запись в /dev/null
```

4. Занимают ли зомби-процессы ресурсы в ОС (CPU, RAM, IO)?  
```

```

6. В IO Visor BCC есть утилита `opensnoop`:

    ```bash
    root@vagrant:~# dpkg -L bpfcc-tools | grep sbin/opensnoop
    /usr/sbin/opensnoop-bpfcc
    ```
    
    На какие файлы вы увидели вызовы группы `open` за первую секунду работы утилиты? Воспользуйтесь пакетом `bpfcc-tools` для Ubuntu 20.04. Дополнительные сведения по установке [по ссылке](https://github.com/iovisor/bcc/blob/master/INSTALL.md).

6. Какой системный вызов использует `uname -a`? Приведите цитату из man по этому системному вызову, где описывается альтернативное местоположение в `/proc` и где можно узнать версию ядра и релиз ОС.
```

```

7. Чем отличается последовательность команд через `;` и через `&&` в bash? Например:

    ```bash
    root@netology1:~# test -d /tmp/some_dir; echo Hi
    Hi
    root@netology1:~# test -d /tmp/some_dir && echo Hi
    root@netology1:~#
    ```
    
    Есть ли смысл использовать в bash `&&`, если применить `set -e`?  
    ```
    
    ```

8. Из каких опций состоит режим bash `set -euxo pipefail`, и почему его хорошо было бы использовать в сценариях?  
```

```

9. Используя `-o stat` для `ps`, определите, какой наиболее часто встречающийся статус у процессов в системе. В `man ps` изучите (`/PROCESS STATE CODES`), что значат дополнительные к основной заглавной букве статуса процессов. Его можно не учитывать при расчёте (считать S, Ss или Ssl равнозначными).
```

```

----

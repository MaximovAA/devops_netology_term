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
Можно восстановить файл и затереть его содержимое
Пример
sudo lsof | grep testfile
cp /proc/2031/fd/2 testfile
echo "" > testfile

Перенаправить &> нужного нам дескриптора в /dev/null
```

4. Занимают ли зомби-процессы ресурсы в ОС (CPU, RAM, IO)?  
```
Нет
```

5. В IO Visor BCC есть утилита `opensnoop`:

    ```bash
    root@vagrant:~# dpkg -L bpfcc-tools | grep sbin/opensnoop
    /usr/sbin/opensnoop-bpfcc
    ```
    
    На какие файлы вы увидели вызовы группы `open` за первую секунду работы утилиты? Воспользуйтесь пакетом `bpfcc-tools` для Ubuntu 20.04. Дополнительные сведения по установке [по ссылке](https://github.com/iovisor/bcc/blob/master/INSTALL.md).
```
библиотеки C:
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3  
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libmagic.so.1", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/liblzma.so.5", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libbz2.so.1.0", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libz.so.1", O_RDONLY|O_CLOEXEC) = 3

root@git:/home/amaksimov# opensnoop-bpfcc -d 1
In file included from <built-in>:2:
In file included from /virtual/include/bcc/bpf.h:12:
In file included from include/linux/types.h:6:
In file included from include/uapi/linux/types.h:14:
In file included from include/uapi/linux/posix_types.h:5:
In file included from include/linux/stddef.h:5:
In file included from include/uapi/linux/stddef.h:5:
In file included from include/linux/compiler_types.h:90:
include/linux/compiler-clang.h:41:9: warning: '__HAVE_BUILTIN_BSWAP32__' macro redefined [-Wmacro-redefined]
#define __HAVE_BUILTIN_BSWAP32__
        ^
<command line>:4:9: note: previous definition is here
#define __HAVE_BUILTIN_BSWAP32__ 1
        ^
In file included from <built-in>:2:
In file included from /virtual/include/bcc/bpf.h:12:
In file included from include/linux/types.h:6:
In file included from include/uapi/linux/types.h:14:
In file included from include/uapi/linux/posix_types.h:5:
In file included from include/linux/stddef.h:5:
In file included from include/uapi/linux/stddef.h:5:
In file included from include/linux/compiler_types.h:90:
include/linux/compiler-clang.h:42:9: warning: '__HAVE_BUILTIN_BSWAP64__' macro redefined [-Wmacro-redefined]
#define __HAVE_BUILTIN_BSWAP64__
        ^
<command line>:5:9: note: previous definition is here
#define __HAVE_BUILTIN_BSWAP64__ 1
        ^
In file included from <built-in>:2:
In file included from /virtual/include/bcc/bpf.h:12:
In file included from include/linux/types.h:6:
In file included from include/uapi/linux/types.h:14:
In file included from include/uapi/linux/posix_types.h:5:
In file included from include/linux/stddef.h:5:
In file included from include/uapi/linux/stddef.h:5:
In file included from include/linux/compiler_types.h:90:
include/linux/compiler-clang.h:43:9: warning: '__HAVE_BUILTIN_BSWAP16__' macro redefined [-Wmacro-redefined]
#define __HAVE_BUILTIN_BSWAP16__
        ^
<command line>:3:9: note: previous definition is here
#define __HAVE_BUILTIN_BSWAP16__ 1
        ^
3 warnings generated.
In file included from <built-in>:2:
In file included from /virtual/include/bcc/bpf.h:12:
In file included from include/linux/types.h:6:
In file included from include/uapi/linux/types.h:14:
In file included from include/uapi/linux/posix_types.h:5:
In file included from include/linux/stddef.h:5:
In file included from include/uapi/linux/stddef.h:5:
In file included from include/linux/compiler_types.h:90:
include/linux/compiler-clang.h:41:9: warning: '__HAVE_BUILTIN_BSWAP32__' macro redefined [-Wmacro-redefined]
#define __HAVE_BUILTIN_BSWAP32__
        ^
<command line>:4:9: note: previous definition is here
#define __HAVE_BUILTIN_BSWAP32__ 1
        ^
In file included from <built-in>:2:
In file included from /virtual/include/bcc/bpf.h:12:
In file included from include/linux/types.h:6:
In file included from include/uapi/linux/types.h:14:
In file included from include/uapi/linux/posix_types.h:5:
In file included from include/linux/stddef.h:5:
In file included from include/uapi/linux/stddef.h:5:
In file included from include/linux/compiler_types.h:90:
include/linux/compiler-clang.h:42:9: warning: '__HAVE_BUILTIN_BSWAP64__' macro redefined [-Wmacro-redefined]
#define __HAVE_BUILTIN_BSWAP64__
        ^
<command line>:5:9: note: previous definition is here
#define __HAVE_BUILTIN_BSWAP64__ 1
        ^
In file included from <built-in>:2:
In file included from /virtual/include/bcc/bpf.h:12:
In file included from include/linux/types.h:6:
In file included from include/uapi/linux/types.h:14:
In file included from include/uapi/linux/posix_types.h:5:
In file included from include/linux/stddef.h:5:
In file included from include/uapi/linux/stddef.h:5:
In file included from include/linux/compiler_types.h:90:
include/linux/compiler-clang.h:43:9: warning: '__HAVE_BUILTIN_BSWAP16__' macro redefined [-Wmacro-redefined]
#define __HAVE_BUILTIN_BSWAP16__
        ^
<command line>:3:9: note: previous definition is here
#define __HAVE_BUILTIN_BSWAP16__ 1
        ^
3 warnings generated.
PID    COMM               FD ERR PATH
494    systemd-oomd        7   0 /proc/meminfo
494    systemd-oomd        7   0 /sys/fs/cgroup/user.slice/user-1000.slice/user@1000.service/memory.pressure
494    systemd-oomd        7   0 /sys/fs/cgroup/user.slice/user-1000.slice/user@1000.service/memory.current
494    systemd-oomd        7   0 /sys/fs/cgroup/user.slice/user-1000.slice/user@1000.service/memory.min
494    systemd-oomd        7   0 /sys/fs/cgroup/user.slice/user-1000.slice/user@1000.service/memory.low
494    systemd-oomd        7   0 /sys/fs/cgroup/user.slice/user-1000.slice/user@1000.service/memory.swap.current
494    systemd-oomd        7   0 /sys/fs/cgroup/user.slice/user-1000.slice/user@1000.service/memory.stat
494    systemd-oomd        7   0 /proc/meminfo
494    systemd-oomd        7   0 /proc/meminfo
494    systemd-oomd        7   0 /proc/meminfo
494    systemd-oomd        7   0 /proc/meminfo
root@git:/home/amaksimov#

```

6. Какой системный вызов использует `uname -a`? Приведите цитату из man по этому системному вызову, где описывается альтернативное местоположение в `/proc` и где можно узнать версию ядра и релиз ОС.
```
/proc/version
              This  string  identifies the kernel version that is currently running.  It includes the con‐
              tents of /proc/sys/kernel/ostype, /proc/sys/kernel/osrelease, and  /proc/sys/kernel/version.
              For example:

                  Linux version 1.0.9 (quinlan@phaze) #1 Sat May 14 01:51:54 EDT 1994


amaksimov@git:~$ cat /proc/version
Linux version 5.19.0-32-generic (buildd@lcy02-amd64-026) (x86_64-linux-gnu-gcc (Ubuntu 11.3.0-1ubuntu1~22.04) 11.3.0, GNU ld (GNU Binutils for Ubuntu) 2.38) #33~22.04.1-Ubuntu SMP PREEMPT_DYNAMIC Mon Jan 30 17:03:34 UTC 2
amaksimov@git:~$ uname -a
Linux git 5.19.0-32-generic #33~22.04.1-Ubuntu SMP PREEMPT_DYNAMIC Mon Jan 30 17:03:34 UTC 2 x86_64 x86_64 x86_64 GNU/Linux


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
    Оператор точка с запятой позволяет запускать несколько команд за один раз, и выполнение команды происходит последовательно.
    Оператор AND (&&) будет выполнять вторую команду только в том случае, если при выполнении первой команды SUCCEEDS, т.е. состояние выхода первой команды равно «0» —     программа выполнена успешно. Эта команда очень полезна при проверке состояния выполнения последней команды.
    
    Команда set с ключом -e  Exit immediately if a command exits with a non-zero status. Таким образом проверка И в данном случае избыточна.
    ```

8. Из каких опций состоит режим bash `set -euxo pipefail`, и почему его хорошо было бы использовать в сценариях?  
```
set -euxo pipefail
-e Exit immediately if a command exits with a non-zero status
-u Treat unset variables as an error when substituting.
-x Print commands and their arguments as they are executed.
-o option-name
pipefail включен, значит статус возврата конвейера — это значение последней (самой правой) команды для выхода с ненулевым статусом или ноль, если все команды завершились успешно. Она, если использовалась опция set -e, не приведёт к аварийному завершению скрипта.
```

9. Используя `-o stat` для `ps`, определите, какой наиболее часто встречающийся статус у процессов в системе. В `man ps` изучите (`/PROCESS STATE CODES`), что значат дополнительные к основной заглавной букве статуса процессов. Его можно не учитывать при расчёте (считать S, Ss или Ssl равнозначными).
```
amaksimov@git:~$ ps -o stat
STAT
Ss
R+
amaksimov@git:~$ ps -O stat
    PID STAT S TTY          TIME COMMAND
   9301 Ss   S pts/0    00:00:00 -bash
  10199 R+   R pts/0    00:00:00 ps -O stat

amaksimov@git:~$ ps -axo stat | wc -l
189
С минимальной погрешностью grep например на заголовок STAT - больше всего в системе спящих процессов
amaksimov@git:~$ ps -axo stat | grep S | wc -l
145

S interruptible sleep (waiting for an event to complete)
s is a session leader
l is multi-threaded (using CLONE_THREAD, like NPTL pthreads do)
```

----

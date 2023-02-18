Домашнее задание к занятию "Работа в терминале. Лекция 1"

Задание

  1.  С помощью базового файла конфигурации запустите Ubuntu 20.04 в VirtualBox посредством Vagrant:

        Создайте директорию, в которой будут храниться конфигурационные файлы Vagrant. В ней выполните vagrant init. Замените содержимое Vagrantfile по умолчанию следующим:

         Vagrant.configure("2") do |config|
         	config.vm.box = "bento/ubuntu-20.04"
         end

        Выполнение в этой директории vagrant up установит провайдер VirtualBox для Vagrant, скачает необходимый образ и запустит виртуальную машину.

        vagrant suspend выключит виртуальную машину с сохранением ее состояния (т.е., при следующем vagrant up будут запущены все процессы внутри, которые работали на момент вызова suspend), vagrant halt выключит виртуальную машину штатным образом.

![setup](https://github.com/MaximovAA/devops_netology_term/blob/main/vagrant%20setup.jpg "Процесс установки и запуска VM")
![setup2](https://github.com/MaximovAA/devops_netology_term/blob/main/install.jpg "Авторизация в VM")

  2.  Ознакомьтесь с графическим интерфейсом VirtualBox, посмотрите как выглядит виртуальная машина, которую создал для вас Vagrant, какие аппаратные ресурсы ей выделены. Какие ресурсы выделены по-умолчанию?

![interface](https://github.com/MaximovAA/devops_netology_term/blob/main/VM%20vagrant.jpg "Конфигурация VM по-умолчанию")

  3.  Ознакомьтесь с возможностями конфигурации VirtualBox через Vagrantfile: документация. Как добавить оперативной памяти или ресурсов процессора виртуальной машине?

![changeconfigVM](https://github.com/MaximovAA/devops_netology_term/blob/main/changeconfigVM.jpg "Изменяем конфиг файл VM по-умолчанию и запускаем VM")

  4.  Команда vagrant ssh из директории, в которой содержится Vagrantfile, позволит вам оказаться внутри виртуальной машины без каких-либо дополнительных настроек. Попрактикуйтесь в выполнении обсуждаемых команд в терминале Ubuntu.

![changeconfigVM](https://github.com/MaximovAA/devops_netology_term/blob/main/vagrantssh.jpg "Пробуем различные команды и смотрим результаты выполнения")

  5.  Ознакомьтесь с разделами man bash, почитайте о настройках самого bash:
        какой переменной можно задать длину журнала history, и на какой строчке manual это описывается?
        что делает директива ignoreboth в bash?

![history_line](https://github.com/MaximovAA/devops_netology_term/blob/main/history_line.jpg "Читаем man")
![History](https://github.com/MaximovAA/devops_netology_term/blob/main/History.jpg "Поиск по ключевым словам")

HISTSIZE — количество команд, которые необходимо запоминать в списке истории (по умолчанию — 500);  
HISTFILESIZE — максимальное количество строк, содержащееся в файле истории ~/.bash_history (по умолчанию — 500);  
export HISTSIZE=10000  
export HISTFILESIZE=10000  

ignorespace — не сохранять строки начинающиеся с символа <пробел>  
ignoredups — не сохранять строки, совпадающие с последней выполненной командой  
ignoreboth — использовать обе опции ‘ignorespace’ и ‘ignoredups’  


  6.  В каких сценариях использования применимы скобки {} и на какой строчке man bash это описано?

![Compounds command](https://github.com/MaximovAA/devops_netology_term/blob/main/Compound%20Commands.jpg "Читаем man")

  7.  С учётом ответа на предыдущий вопрос, как создать однократным вызовом touch 100000 файлов? Получится ли аналогичным образом создать 300000? Если нет, то почему?

  8.  В man bash поищите по /\[\[. Что делает конструкция [[ -d /tmp ]]

![expression](https://github.com/MaximovAA/devops_netology_term/blob/main/expression.jpg "Поиск в man bash /")
![Matched](https://github.com/MaximovAA/devops_netology_term/blob/main/Matched.jpg "Проверяем условие [[ -d /tmp ]] ")

  9.  Сделайте так, чтобы в выводе команды type -a bash первым стояла запись с нестандартным путем, например bash is ... Используйте знания о просмотре существующих и создании новых переменных окружения, обратите внимание на переменную окружения PATH

    bash is /tmp/new_path_directory/bash
    bash is /usr/local/bin/bash
    bash is /bin/bash

    (прочие строки могут отличаться содержимым и порядком) В качестве ответа приведите команды, которые позволили вам добиться указанного вывода или соответствующие скриншоты.
![PATH](https://github.com/MaximovAA/devops_netology_term/blob/main/Path.jpg "Создаем директорию, копируем туда bash, меняем PATH через export")

  10. Чем отличается планирование команд с помощью batch и at?
  
  at выполняет команды в указанное время, batch выполняет команды, когда позволяют уровни загрузки системы  
  
![PATH](https://github.com/MaximovAA/devops_netology_term/blob/main/at_batch.jpg "man batch")

  11.  Завершите работу виртуальной машины чтобы не расходовать ресурсы компьютера и/или батарею ноутбука.

  vagrant halt

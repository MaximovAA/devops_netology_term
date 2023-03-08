------

## Задание

1. Узнайте о [sparse-файлах](https://ru.wikipedia.org/wiki/%D0%A0%D0%B0%D0%B7%D1%80%D0%B5%D0%B6%D1%91%D0%BD%D0%BD%D1%8B%D0%B9_%D1%84%D0%B0%D0%B9%D0%BB) (разряженных).
```
sparse file — файл, в котором последовательности нулевых байтов заменены на информацию об этих последовательностях
```

2. Могут ли файлы, являющиеся жёсткой ссылкой на один объект, иметь разные права доступа и владельца? Почему?
```
В Linux каждый файл имеет уникальный идентификатор - индексный дескриптор (inode). Это число, которое однозначно идентифицирует файл в файловой системе. Жесткая ссылка и файл, для которой она создавалась имеют одинаковые inode. Поэтому жесткая ссылка имеет те же права доступа, владельца и время последней модификации, что и целевой файл. Различаются только имена файлов. Фактически жесткая ссылка это еще одно имя для файла.
```

3. Сделайте `vagrant destroy` на имеющийся инстанс Ubuntu. Замените содержимое Vagrantfile следующим:

    ```ruby
    path_to_disk_folder = './disks'

    host_params = {
        'disk_size' => 2560,
        'disks'=>[1, 2],
        'cpus'=>2,
        'memory'=>2048,
        'hostname'=>'sysadm-fs',
        'vm_name'=>'sysadm-fs'
    }
    Vagrant.configure("2") do |config|
        config.vm.box = "bento/ubuntu-20.04"
        config.vm.hostname=host_params['hostname']
        config.vm.provider :virtualbox do |v|

            v.name=host_params['vm_name']
            v.cpus=host_params['cpus']
            v.memory=host_params['memory']

            host_params['disks'].each do |disk|
                file_to_disk=path_to_disk_folder+'/disk'+disk.to_s+'.vdi'
                unless File.exist?(file_to_disk)
                    v.customize ['createmedium', '--filename', file_to_disk, '--size', host_params['disk_size']]
                end
                v.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', disk.to_s, '--device', 0, '--type', 'hdd', '--medium', file_to_disk]
            end
        end
        config.vm.network "private_network", type: "dhcp"
    end
    ```

    Эта конфигурация создаст новую виртуальную машину с двумя дополнительными неразмеченными дисками по 2,5 Гб.
```
Выполнено
vagrant up
```

4. Используя `fdisk`, разбейте первый диск на два раздела: 2 Гб и оставшееся пространство.
```
Смотрим lsblk, выбираем нужный раздел
sudo fdisk /dev/sdb
Выставляем параметры в мастере, по аналогии разбиваем оставшееся место
```
![fdisk](https://github.com/MaximovAA/devops_netology_term/blob/main/fdisk.jpg "Пример вывода команд")

5. Используя `sfdisk`, перенесите эту таблицу разделов на второй диск.
```
sfdisk -d /dev/sdb | sfdisk /dev/sdc
```
![sfdisk](https://github.com/MaximovAA/devops_netology_term/blob/main/sfdisk.jpg "Пример вывода команд")

6. Соберите `mdadm` RAID1 на паре разделов 2 Гб.
```
sudo mdadm --create --verbose /dev/md1 --level=1 --raid-devices=2 /dev/sd[b-c]1
```

7. Соберите `mdadm` RAID0 на второй паре маленьких разделов.
```
sudo mdadm --create --verbose /dev/md0 --level=0 --raid-devices=2 /dev/sd[b-c]2
```

8. Создайте два независимых PV на получившихся md-устройствах.
```
vagrant@sysadm-fs:~$ sudo pvcreate /dev/md0
  Physical volume "/dev/md0" successfully created.
vagrant@sysadm-fs:~$ sudo pvcreate /dev/md1
  Physical volume "/dev/md1" successfully created.
  
sudo pvdisplay
```

9. Создайте общую volume-group на этих двух PV.
```
vagrant@sysadm-fs:~$ sudo vgcreate vg01 /dev/md0 /dev/md1
  Volume group "vg01" successfully created
 
sudo vgdisplay
```

10. Создайте LV размером 100 Мб, указав его расположение на PV с RAID0.
```
sudo lvcreate -L100 -n lv01 vg01 /dev/md0

sudo lvdisplay
```

11. Создайте `mkfs.ext4` ФС на получившемся LV.
```
sudo mkfs.ext4 /dev/vg01/lv01
```

12. Смонтируйте этот раздел в любую директорию, например, `/tmp/new`.
```
sudo mount /dev/vg01/lv01 /tmp/new
```

13. Поместите туда тестовый файл, например, `wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz`.
```
sudo wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz
```

14. Прикрепите вывод `lsblk`.
```
vagrant@sysadm-fs:~$ lsblk
NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
loop0                       7:0    0 49.9M  1 loop  /snap/snapd/18357
loop1                       7:1    0 67.8M  1 loop  /snap/lxd/22753
loop2                       7:2    0 91.9M  1 loop  /snap/lxd/24061
loop3                       7:3    0 63.3M  1 loop  /snap/core20/1828
loop4                       7:4    0   62M  1 loop  /snap/core20/1611
sda                         8:0    0   64G  0 disk
├─sda1                      8:1    0    1M  0 part
├─sda2                      8:2    0    2G  0 part  /boot
└─sda3                      8:3    0   62G  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0   31G  0 lvm   /
sdb                         8:16   0  2.5G  0 disk
├─sdb1                      8:17   0    2G  0 part
│ └─md1                     9:1    0    2G  0 raid1
└─sdb2                      8:18   0  510M  0 part
  └─md0                     9:0    0 1016M  0 raid0
    └─vg01-lv01           253:1    0  100M  0 lvm   /tmp/new
sdc                         8:32   0  2.5G  0 disk
├─sdc1                      8:33   0    2G  0 part
│ └─md1                     9:1    0    2G  0 raid1
└─sdc2                      8:34   0  510M  0 part
  └─md0                     9:0    0 1016M  0 raid0
    └─vg01-lv01           253:1    0  100M  0 lvm   /tmp/new
```

15. Протестируйте целостность файла:

    ```bash
    root@vagrant:~# gzip -t /tmp/new/test.gz
    root@vagrant:~# echo $?
    0
    ```
```
vagrant@sysadm-fs:~$ gzip -t /tmp/new/test.gz
vagrant@sysadm-fs:~$ echo $?
0
```

16. Используя pvmove, переместите содержимое PV с RAID0 на RAID1.
```
vagrant@sysadm-fs:~$ sudo pvmove /dev/md0 /dev/md1
  /dev/md0: Moved: 20.00%
  /dev/md0: Moved: 100.00%
```

17. Сделайте `--fail` на устройство в вашем RAID1 md.
```
sudo mdadm /dev/md1 --fail /dev/sdc1
```

18. Подтвердите выводом `dmesg`, что RAID1 работает в деградированном состоянии.
```
[ 6781.611304] md/raid1:md1: Disk failure on sdc1, disabling device.
               md/raid1:md1: Operation continuing on 1 devices.
```

19. Протестируйте целостность файла — он должен быть доступен несмотря на «сбойный» диск:

    ```bash
    root@vagrant:~# gzip -t /tmp/new/test.gz
    root@vagrant:~# echo $?
    0
    ```
```
vagrant@sysadm-fs:~$ gzip -t /tmp/new/test.gz
vagrant@sysadm-fs:~$ echo $?
0
```

20. Погасите тестовый хост — `vagrant destroy`.
```
vagrant destroy
```

----

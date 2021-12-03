# Домашнее задание к занятию "3.4. Операционные системы, лекция 2"

# Задание 1  
Архив `node_exporter` скачан, распакован в папку `/home/user/node_exporter`
запущен через `./node_exporter` - работает на `localhost:9100`. Остановлен.

Создание юнит-файла для systemd (со ссылкой на внешний файл переменных окружения):  
~~~
sudo nano /etc/systemd/system/nodeexporter.service
~~~
Содержимое файла:
~~~
[Unit]
Description=node_exporter

[Service]
ExecStart=/home/user/node_exporter/node_exporter $OPTIONS
EnvironmentFile=/home/user/node_exporter/myenv.file

[Install]
WantedBy=multi-user.target
~~~
Дополнительную информацию можно передать в node_exporter в переменной $OPTIONS в качестве аргумента в строке ExecStart=
Создаем файл c переменной окружения:  
~~~
echo MyTextVar=\'My external variable in file\' > myenv.file
~~~
Запуск (успешно):
~~~
sudo systemctl daemon-reload
sudo systemctl start nodeexporter
~~~
Останов (успешно):  
~~~
sudo systemctl stop nodeexporter
~~~
Добавление в автозагрузку (успешно, после рестарта VM все поднимается):
~~~
sudo systemctl enable nodeexporter
~~~
Проверка появившейся переменной окружения в процессе (3694):  
~~~
user@vboxpc:~$ ps aux | grep export
root        3694  0.0  0.3 717892 15160 ?        Ssl  10:49   0:00 /home/user/node_exporter/node_exporter
~~~
Смотрим переменную в /proc:  
~~~
user@vboxpc:~$ sudo cat /proc/3694/environ
LANG=en_US.UTF-8LC_ADDRESS=ru_RU.UTF-8LC_IDENTIFICATION=ru_RU.UTF-8LC_MEASUREMENT=ru_RU.UTF-8LC_MONETARY=ru_RU.UTF-8LC_NAME=ru_RU.UTF-8LC_NUMERIC=ru_RU.UTF-8LC_PAPER=ru_RU.UTF-8LC_TELEPHONE=ru_RU.UTF-8LC_TIME=ru_RU.UTF-8PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/binINVOCATION_ID=3b77dc4ebbc2411e9814656e8d335725JOURNAL_STREAM=8:48901MyTextVar=My external variable in file
~~~
последняя в строке.  
# Задание 2
Выбрал следующие параметры для мониторинга:  
CPU:  
~~~
node_cpu_seconds_total{cpu="0",mode="system"}
node_cpu_seconds_total{cpu="0",mode="user"}
node_cpu_seconds_total{cpu="1",mode="system"}
node_cpu_seconds_total{cpu="1",mode="user"}
node_cpu_seconds_total{cpu="2",mode="system"}
node_cpu_seconds_total{cpu="2",mode="user"}
node_cpu_seconds_total{cpu="3",mode="system"}
node_cpu_seconds_total{cpu="3",mode="user"}
~~~
Для памяти:  
~~~
node_memory_MemAvailable_bytes 
node_memory_MemFree_bytes
~~~
Для дисков (Для моей конфигурации):  
~~~
node_filesystem_avail_bytes{device="/dev/sda5",fstype="ext4",mountpoint="/"}
node_filesystem_free_bytes{device="/dev/sda5",fstype="ext4",mountpoint="/"}
~~~
Для сети:
~~~
node_network_receive_bytes_total{device="enp0s3"} 
node_network_transmit_bytes_total{device="enp0s3"}
~~~
# Задание 3
Пакет `netdata` на виртуальную машину установлен (работает)  
Конфигурационный файл исправлен, после перезагрузки виртуальной машины успешно произведено подключение к серверу `netdata` с хостовой машины на порт 19999.
С метриками `netdata` ознакомился.  
# Задание 4
Вот что нашлось по поиску `virt` в выводе dmesg:
~~~
user@vboxpc:~$ dmesg | grep virt
[    0.003887] CPU MTRRs all blank - virtualized system.
[    0.202153] Booting paravirtualized kernel on KVM
[    5.618510] systemd[1]: Detected virtualization oracle.
~~~
В первом сообщении видно что все регистры MTTR пустые, они судя по ВИКИ используются для более оптимального взаимодействия с различными типами физической памяти  
[ВИКИ MTTR](https://ru.wikipedia.org/wiki/MTRR)  
думаю уже поэтому можно предположить (как и по двум другим сообщениям), что ОС видит, что окружение виртуальное, по крайней мере на гипервизоре VirtualBox.
# Задание 5
Из официального описания:  
`nr_open` - максимальное количество дескрипторов файлов, которые может выделить процесс. Значение по умолчанию 1024*1024 (1048576), которого должно быть достаточно для большинства машин. Фактический лимит зависит от лимита ресурсов RLIMIT_NOFILE.  
на моей машине:  
~~~
user@vboxpc:~$ sysctl fs.nr_open
fs.nr_open = 1048576
~~~
Еще один ограничитель (из `ulimit --help`):  
~~~
user@vboxpc:~$ ulimit -n
1024
~~~
-n	максимальное кол-во открытых файловых дескрипторов (1024 штуки)
# Задание 6
Запускаем 1й терминал (screen не установлен):  
~~~
user@vboxpc:~$ sudo unshare -f --pid --mount-proc sleep 1h
~~~
Запускаем 2й терминал:  
~~~
user@vboxpc:~$ sudo nsenter --target 4653 --pid --mount
[sudo] password for user: 
root@vboxpc:/# ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0   8088   588 pts/1    S+   15:58   0:00 sleep 1h
root           2  0.6  0.1  10880  5052 pts/0    S    15:59   0:00 -bash
root          11  0.0  0.0  11504  3340 pts/0    R+   15:59   0:00 ps aux
~~~
Имеем `sleep 1h` c PID 1 в новом пространстве имен.
# Задание 7
Указанная последовательность - "логическая бомба", код которой создаёт функцию, которая запускает ещё два своих экземпляра, которые, в свою очередь снова запускают эту функцию и так до тех пор, пока этот процесс не займёт всю физическую память компьютера (если заранее не указать ограничения).  
При настройках по умолчанию виртуальная машина висла намертво. CPU = 100%, забивается основная память до 97%, затем забивается своп файл на 100%, какое то время в термиле валятся ошибки типа `bash: fork: retry: Resource temporarily unavailable` потом все зависает.    
после ограничений кол-ва создаваемых процессов по `ulimit -u 100` скрипт заканчивал работу через некоторое время.  
в `dmesg` ничего полезного не нашел, пытался в разных последовательностях. Возможно разница в том что 20.04 установлена не через vagrant.  
Стабилизация как я понимаю должна произойти после достижения определенного порога создаваемых процессов, после чего ядро должно отклонять запросы на их создание.

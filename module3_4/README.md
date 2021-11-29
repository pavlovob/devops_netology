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
ExecStart=/home/user/node_exporter/node_exporter
EnvironmentFile=/home/user/node_exporter/myenv.file

[Install]
WantedBy=multi-user.target
~~~
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
3. Как настроен sysctl `fs.nr_open` на системе по-умолчанию? Узнайте, что означает этот параметр. Какой другой существующий лимит не позволит достичь такого числа (`ulimit --help`)?
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
5. Запустите любой долгоживущий процесс (не `ls`, который отработает мгновенно, а, например, `sleep 1h`) в отдельном неймспейсе процессов; покажите, что ваш процесс работает под PID 1 через `nsenter`. Для простоты работайте в данном задании под root (`sudo -i`). Под обычным пользователем требуются дополнительные опции (`--map-root-user`) и т.д.
# Задание 7
7. Найдите информацию о том, что такое `:(){ :|:& };:`. Запустите эту команду в своей виртуальной машине Vagrant с Ubuntu 20.04 (**это важно, поведение в других ОС не проверялось**). Некоторое время все будет "плохо", после чего (минуты) – ОС должна стабилизироваться. Вызов `dmesg` расскажет, какой механизм помог автоматической стабилизации. Как настроен этот механизм по-умолчанию, и как изменить число процессов, которое можно создать в сессии?

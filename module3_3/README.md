# Домашнее задание к занятию "3.3. Операционные системы, лекция 1"

# Задание 1
Запуск `strace /bin/bash -c 'cd /tmp'` выдал хронологию системных вызовов bash. в конце списка вызовов есть команда `chdir("/tmp")` (5 строчка снизу):
![res](img/img1.png)
# Задание 2
Посмотрел по `grep` системного вызова `openat` для открытия файлов. Выполнил команды
~~~
strace file /dev/sda 2>tmp
cat tmp | grep open
~~~
В конце списка открываемых файлов есть 3 файла  
/etc/magic.mgc (не существует)  
/etc/magic  
/usr/share/misc/magic.mgc  
![res](img/img2.png)  
похоже, что это и есть БД функции `file`. в файле `/usr/share/misc/magic.mgc` есть много данных. в `man` по функции написано что можно добавлять свои определения в файл `/etc/magic`
# Задание 3
- Запускаем vim с somefile,  набираем что-нибудь, сохраняем, из vim не выходим, удаляем somefile из другого процесса (терминала)
- ps: ищем процесс (67599)  
- lsof: находим открытый процессом файл (fd\3)  
- перезаписываем "ничего" в дескриптор файла (3)  
- смотрим еще раз, размер файла = 0  
код:  
~~~
vi somefile
ps aux | grep somefile
lsof -p  67599
> /proc/67599/fd/3
lsof -p 67599
~~~
# Задание 4
Зомби процессы завершаются полностью, оставляя после себя только запись в таблице процессов.
# Задание 5
Вывод утилиты за 1 секунду работы (параметр -d 1):
~~~
vagrant@developer:~$ sudo /usr/sbin/opensnoop-bpfcc -d 1
PID    COMM               FD ERR PATH
864    vminfo              4   0 /var/run/utmp
708    dbus-daemon        -1   2 /usr/local/share/dbus-1/system-services
708    dbus-daemon        56   0 /usr/share/dbus-1/system-services
708    dbus-daemon        -1   2 /lib/dbus-1/system-services
708    dbus-daemon        56   0 /var/lib/snapd/dbus-1/system-services/
4581   ThreadPoolSingl   245   0 /proc/6721/oom_score_adj
4581   ThreadPoolForeg   245   0 /tmp/.com.google.Chrome.S8lYAB
4581   ThreadPoolForeg   245   0 /tmp/.com.google.Chrome.S8lYAB
~~~
# Задание 6
Команда `uname -a` использует вызов системной функции `uname`:  
~~~
close(3)                                = 0
uname({sysname="Linux", nodename="developer", ...}) = 0
fstat(1, {st_mode=S_IFCHR|0620, st_rdev=makedev(0x88, 0), ...}) = 0
uname({sysname="Linux", nodename="developer", ...}) = 0
uname({sysname="Linux", nodename="developer", ...}) = 0
write(1, "Linux developer 5.4.0-90-generic"..., 109Linux developer 5.4.0-90-generic #101-Ubuntu SMP Fri Oct 15 20:00:55 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
) = 109
close(1)                                = 0
close(2)                                = 0
exit_group(0)                           = ?
+++ exited with 0 +++
vagrant@developer:~$ 
~~~
1. Какой системный вызов использует `uname -a`? Приведите цитату из man по этому системному вызову, где описывается альтернативное местоположение в `/proc`, где можно узнать версию ядра и релиз ОС.
# Задание 7
3. Чем отличается последовательность команд через `;` и через `&&` в bash? Например:
    ```bash
    root@netology1:~# test -d /tmp/some_dir; echo Hi
    Hi
    root@netology1:~# test -d /tmp/some_dir && echo Hi
    root@netology1:~#
    ```
    Есть ли смысл использовать в bash `&&`, если применить `set -e`?
# Задание 8
1. Из каких опций состоит режим bash `set -euxo pipefail` и почему его хорошо было бы использовать в сценариях?
# Задание 9
1. Используя `-o stat` для `ps`, определите, какой наиболее часто встречающийся статус у процессов в системе. В `man ps` ознакомьтесь (`/PROCESS STATE CODES`) что значат дополнительные к основной заглавной буквы статуса процессов. Его можно не учитывать при расчете (считать S, Ss или Ssl равнозначными).

 
 ---

## Как сдавать задания

Обязательными к выполнению являются задачи без указания звездочки. Их выполнение необходимо для получения зачета и диплома о профессиональной переподготовке.

Задачи со звездочкой (*) являются дополнительными задачами и/или задачами повышенной сложности. Они не являются обязательными к выполнению, но помогут вам глубже понять тему.

Домашнее задание выполните в файле readme.md в github репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.

Также вы можете выполнить задание в [Google Docs](https://docs.google.com/document/u/0/?tgif=d) и отправить в личном кабинете на проверку ссылку на ваш документ.
Название файла Google Docs должно содержать номер лекции и фамилию студента. Пример названия: "1.1. Введение в DevOps — Сусанна Алиева".

Если необходимо прикрепить дополнительные ссылки, просто добавьте их в свой Google Docs.

Перед тем как выслать ссылку, убедитесь, что ее содержимое не является приватным (открыто на комментирование всем, у кого есть ссылка), иначе преподаватель не сможет проверить работу. Чтобы это проверить, откройте ссылку в браузере в режиме инкогнито.

[Как предоставить доступ к файлам и папкам на Google Диске](https://support.google.com/docs/answer/2494822?hl=ru&co=GENIE.Platform%3DDesktop)

[Как запустить chrome в режиме инкогнито ](https://support.google.com/chrome/answer/95464?co=GENIE.Platform%3DDesktop&hl=ru)

[Как запустить  Safari в режиме инкогнито ](https://support.apple.com/ru-ru/guide/safari/ibrw1069/mac)

Любые вопросы по решению задач задавайте в чате Slack.

---

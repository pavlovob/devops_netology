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
Цитата из секции NOTES (замечания) man 2 касательно альтернативного расположения информации о версии ядра и т.д.:  
~~~
 Часть  информации,  возвращаемой  данным  системным  вызовом также доступна через sysctl и
       через файлы /proc/sys/kernel/{ostype, hostname, osrelease, version, domainname}.
~~~
# Задание 7
символ `;` простой разделитель последовательного выполненния команд
~~~
test -d /tmp/some_dir; echo Hi
~~~
- последовательно выполнит укащанные команды вне зависимости от результата каждой 
символ `&&` логический оперитор типа `И`
~~~
test -d /tmp/some_dir && echo Hi 
~~~
- echo выполнится только при успешном выполнении команды test  
т.к.при использовании условия выполнения команд `&&` происходит останов выполнения при наличии ошибки выполнения предыдущей команды, `set -e` в данном случае выполнять смысла нет, т.к. она так же прерывает выполнение при ошибках (не нулевое значение возврата) кроме последней команды.  
# Задание 8  
По man:  
-e останов выполнения дальнейших команд в пайпе за исключением последней команды условий, циклов и пр.  
-x выводит значения аргументов расширяемых команд (простые,for, case, select) с значениями результатов их выполнения.  
-u возврат ненулевого кода выхода при встрече с неустановленной переменной (параментра) командой set (кроме спец парамера @ и *). Если не интерактивно, вывод в stderr текста, выход с не 0 кодом.  
-o дополнитлеьная опция. Прааметр `pipefail` возвращает значение самой правой команды в пайпе, завершившейся без ошибки или 0 если все завершилось нормально. (о умолчанию отключено)  
bash сценарии с подобными опциями рекомендуется использовать для отладки скриптов (если что, сразу выход со значениями).
# Задание 9
~~~
Список сотояний:  
vagrant@developer:~$ ps -o stat  
STAT
Ss
R+
~~~
S - interruptible sleep (waiting for an event to complete)   
R - running or runnable (on run queue)   
Дополнительные буквы статусов:  
`s` - session leader  
`+` - находится в группе процессов переднего плана  

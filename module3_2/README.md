Домашнее задание к модулю 3_1 
------------

### Задание 1. 
Команда cd является именно КОМАНДОЙ оболочки а не программой или скриптом. Так как результат работы команды - просто смена указателя каталога файловой системы, нет смысла делать ее внешней, т.к. в этом случае было бы необходимо создавать дополнительный подпроцесс оболочки и в этом случае, смена указателя происходила бы именно в том, новом подпроцессе а не в родительском. При выходе из порождаемого процесса указатель на каталог все равно остался бы на прежнем месте родительского процесса оболочки. Это можно легко проверить, написав скрипт cd /dir1/dir2/ и запустив его смены каталога в вызывающей скрипт оболочке не произойдет. На сколько я понял, на более низком уровне команда cd запускает POSIX совместимую функцию СИ chdir(). 

### Задание 2.
Альтернативой без pipe команде 
~~~
grep <some_string> <some_file> | wc -l
~~~
является  
~~~
grep -с <some_string> <so### 7.me_file>
~~~
Параметр -с указывает на изменение стандартного вывода информации команды на вывод количества подсчитанных строк совпадений поиска.
С документацией по bad practices in pipes по ссылке ознакомлен.  
### Задание 3.
Процесс 'systemd' с PID 1 является родителем для всех процессов в Ubuntu 20.04
### Задание 4.
Командой перенаправления вывода ошибок в другую сесиию терминала (в pts1 при нахождении в терминале pts0, например) будет:  
~~~
ls -l /bin/usr 2> /dev/pts/1
~~~
каталог /bin/usr естественно, не существует.
### Задание 5.
Думаю такое возможно. Что с ходу пришло в голову:
~~~
user@wbn04pavlov:~/tmp$ ls -l > file1
user@wbn04pavlov:~/tmp$ cat <file1 1>file2
user@wbn04pavlov:~/tmp$ cat file2
total 0
-rw-rw-r-- 1 user user 0 ноя 18 11:57 file1
~~~
сформировали файл, указали вход и выход, посмотрели что получилось.
### Задание 6.
Гипотетически, сделать это скорее всего можно. Команда должна отправить из текущего PTY в tty4 HELLO:
~~~
echo HELLO >/dev/tty4
~~~
Переключившись в tty4 (по ctrl+alt+F4) можно посмотреть что пришло и отправить обратно что-то в PTY:
~~~
echo HELLO >/dev/pts/0
~~~
на деле, TTY и PTS в Ubuntu 20.04, похоже, иззолированы и возвращается ошибка "Недостаточно прав" в том числе и под sudo:
~~~
user@wbn04pavlov:~/tmp$ ls -l >/dev/tty3
bash: /dev/tty3: Permission denied
user@wbn04pavlov:~/tmp$ sudo ls -l >/dev/tty3
bash: /dev/tty3: Permission denied
~~~
Permission denied выдается и на standalone ubuntu 20.04, ssh сессии терминала vagrant-VBox и в консоли VBox Ubuntu 20.04
между tty - tty и pty - pty сообщения передаются.
### Задание 7.
Команда bash 5>&1 создаст новый файловый дескриптор с указанием на stdout (используется для сохранения потоков). команда echo netology > /proc/$$/fd/5 направит сообщение netology в файл с дескриптором 5. Т.к. он ссылается на stdout (поток 1), по умолчанию сообщение будет выведено на экране.
### Задание 8.
Думаю, так можно сделать. Примерно так:
~~~
user@wbn04pavlov:~$ ls -l /usr/bin1 7>&2 2>&1 1>&7 |grep no -c
1
~~~
Каталог /usr/bin1 заведомо несуществует, команда вернет ls: cannot access '/usr/bin1': No such file or directory  
При этом стандартный вывод будет рабоать.
### Задание 9.
Команда 
~~~
cat /proc/$$/environ
~~~
Выведет все переменные окружения для текущей сессии командного интерпретатора.
Аналогичный по содержанию вывод можно молучить командами
~~~
env
printenv
~~~
### Задание 10.
По адресу (строка 171 man):
~~~
/proc/<PID>/cmdline
~~~
Доступен файл только для чтения, в котором указана полная командная строка запущенного процесса (если процесс не зомби). Аргументы так же находятся в этом файле в виде набора строк разделенных байтом ('\0').  
По адресу (строка 211 man):
~~~
/proc/<PID>/exe
~~~
Доступен файл (символическая ссылка) с указаанием актуального пути исполняемой команды. Попытка его открыть приведет к запуску команды. 
### Задание 11.
Версия SSE моего ПК - 4.2:
~~~
user@wbn04pavlov:/proc/72982$ cat /proc/cpuinfo | grep sse
~~~
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc art arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc cpuid aperfmperf pni pclmulqdq dtes64 monitor ds_cpl vmx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4_1 <strong>sse4_2</strong> x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm 3dnowprefetch cpuid_fault epb invpcid_single pti ssbd ibrs ibpb stibp tpr_shadow vnmi flexpriority ept vpid ept_ad fsgsbase tsc_adjust bmi1 avx2 smep bmi2 erms invpcid mpx rdseed adx smap clflushopt intel_pt xsaveopt xsavec xgetbv1 xsaves dtherm ida arat pln pts hwp hwp_notify hwp_act_window hwp_epp md_clear flush_l1d

~~~
### Задание 12.
Команда 
~~~
vagrant@netology1:~$ ssh localhost 'tty'
~~~
Выводит ошибку "not a tty" из-за того, что ssh, в случае обнаружения команд в параметрах командной строки, которые должны быть выполнены на удаленной машине сразу после подключения не инициализирует по умолчанию TTY т.к. подразумевает отсутствие интерактивного входа, а только исполнение команд (скрипта) и возврат. 
Для того, чтобы принудительно инициализировать TTY при передаче команд в ssh необходимо установить флаг -t (или -ttб без проверки наличия локального TTY). 
~~~
ssh -t localhost 'tty'
~~~
### Задание 13.
### Задание 14.

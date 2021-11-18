Домашнее задание к модулю 3_1 
------------

### 1. 
Команда cd является именно КОМАНДОЙ оболочки а не программой или скриптом. Так как результат работы команды - просто смена указателя каталога файловой системы, нет смысла делать ее внешней, т.к. в этом случае было бы необходимо создавать дополнительный подпроцесс оболочки и в этом случае, смена указателя происходила бы именно в том, новом подпроцессе а не в родительском. При выходе из порождаемого процесса указатель на каталог все равно остался бы на прежнем месте родительского процесса оболочки. Это можно легко проверить, написав скрипт cd /dir1/dir2/ и запустив его смены каталога в вызывающей скрипт оболочке не произойдет. На сколько я понял, на более низком уровне команда cd запускает POSIX совместимую функцию СИ chdir(). 

### 2.
Альтернативой без pipe команде 
~~~
grep <some_string> <some_file> | wc -l
~~~
является  
~~~
grep -с <some_string> <some_file>
~~~
Параметр -с указывает на изменение стандартного вывода информации команды на вывод количества подсчитанных строк совпадений поиска.
С документацией по bad practices in pipes по ссылке ознакомлен.  
### 3.
Процесс 'systemd' с PID 1 является родителем для всех процессов в Ubuntu 20.04
### 4.
Командой перенаправления вывода ошибок в другую сесиию терминала (в pts1 при нахождении в терминале pts0, например) будет:  
~~~
ls -l /bin/usr 2> /dev/pts/1
~~~
каталог /bin/usr естественно, не существует.
### 5.
Думаю такое возможно. Что с ходу пришло в голову:
~~~
user@wbn04pavlov:~/tmp$ ls -l > file1
user@wbn04pavlov:~/tmp$ cat <file1 1>file2
user@wbn04pavlov:~/tmp$ cat file2
total 0
-rw-rw-r-- 1 user user 0 ноя 18 11:57 file1
~~~
сформировали файл, указали вход и выход, посмотрели что получилось.
### 6.
Гипотетически, сделать это скорее всего можно. Команда
~~~
echo HELLO >/dev/tty4
~~~
Должна отправить из текущего PTY в tty4 HELLO
А переключившись в tty4 (по ctrl+alt+F4) можно отправить что то в PTY
~~~
echo HELLO /dev/pts/0
~~~
на деле, TTY и PTS в Ubuntu 20.04, похоже, иззолированы и возвращается ошибка "Недостаточно прав":
~~~
user@wbn04pavlov:~/tmp$ ls -l >/dev/tty3
bash: /dev/tty3: Permission denied
user@wbn04pavlov:~/tmp$ sudo ls -l >/dev/tty3
bash: /dev/tty3: Permission denied
~~~



If you do not have [Composer](http://getcomposer.org/), you may install it by following the instructions

at [getcomposer.org](http://getcomposer.org/doc/00-intro.md#installation-nix).

You can then install this project template using the following command:

~~~
composer create-project --prefer-dist yiisoft/yii2-app-basic basic
~~~

Now you should be able to access the application through the following URL, assuming `basic` is the directory
directly under the Web root.

~~~
http://localhost/basic/web/
~~~

### Install from an Archive File

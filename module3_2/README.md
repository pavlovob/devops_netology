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

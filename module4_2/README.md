## Обязательная задача 1

Есть скрипт:
```python
#!/usr/bin/env python3
a = 1
b = '2'
c = a + b
```

### Вопросы:
| Вопрос  | Ответ |
| ------------- | ------------- |
| Какое значение будет присвоено переменной `c`?  | Ошибка несоответствия типов при сложении |
| Как получить для переменной `c` значение 12?  | c = str(a) + b |
| Как получить для переменной `c` значение 3?  | c = a + int(b)  |

## Обязательная задача 2
Мы устроились на работу в компанию, где раньше уже был DevOps Engineer. Он написал скрипт, позволяющий узнать, какие файлы модифицированы в репозитории, относительно локальных изменений. Этим скриптом недовольно начальство, потому что в его выводе есть не все изменённые файлы, а также непонятен полный путь к директории, где они находятся. Как можно доработать скрипт ниже, чтобы он исполнял требования вашего руководителя?

```python
#!/usr/bin/env python3

import os

bash_command = ["cd ~/netology/sysadm-homeworks", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
        break
```

### Ваш скрипт:
```python
#!/usr/bin/env python3

import os

bash_command = ["cd ~/netology/sysadm-homeworks",  "pwd", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
cur_dir = result_os.split('\n')[0]
is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(cur_dir + "/" + prepare_result)
        #break
```

### Вывод скрипта при запуске при тестировании:
```
user@wbn04pavlov:~/tmp$ ./file1
/home/user/netology/sysadm-homeworks/3
/home/user/netology/sysadm-homeworks/5
```

## Обязательная задача 3
1. Доработать скрипт выше так, чтобы он мог проверять не только локальный репозиторий в текущей директории, а также умел воспринимать путь к репозиторию, который мы передаём как входной параметр. Мы точно знаем, что начальство коварное и будет проверять работу этого скрипта в директориях, которые не являются локальными репозиториями.

### Ваш скрипт:
```python
???
```

### Вывод скрипта при запуске при тестировании:
```
???
```

## Обязательная задача 4
1. Наша команда разрабатывает несколько веб-сервисов, доступных по http. Мы точно знаем, что на их стенде нет никакой балансировки, кластеризации, за DNS прячется конкретный IP сервера, где установлен сервис. Проблема в том, что отдел, занимающийся нашей инфраструктурой очень часто меняет нам сервера, поэтому IP меняются примерно раз в неделю, при этом сервисы сохраняют за собой DNS имена. Это бы совсем никого не беспокоило, если бы несколько раз сервера не уезжали в такой сегмент сети нашей компании, который недоступен для разработчиков. Мы хотим написать скрипт, который опрашивает веб-сервисы, получает их IP, выводит информацию в стандартный вывод в виде: <URL сервиса> - <его IP>. Также, должна быть реализована возможность проверки текущего IP сервиса c его IP из предыдущей проверки. Если проверка будет провалена - оповестить об этом в стандартный вывод сообщением: [ERROR] <URL сервиса> IP mismatch: <старый IP> <Новый IP>. Будем считать, что наша разработка реализовала сервисы: `drive.google.com`, `mail.google.com`, `google.com`.

### Ваш скрипт:
```python
#!/usr/bin/env python3

import os
import argparse

ps = argparse.ArgumentParser()
ps.add_argument("path", nargs = "?", help = "Path to git repo...")
args = ps.parse_args()
if not args.path or not os.path.isdir(args.path):
    print("Path not set or does not exist, using current folder...")
    cur_dir = os.getcwd()
else:
    cur_dir = args.path
    
bash_command = ["cd " + cur_dir, "git 2>/dev/null status "]
result_os = os.popen(' && '.join(bash_command)).read()
if result_os.split('\n')[0].find('fatal: not') != -1 :
  print("No git repo exist on path: " + cur_dir)
  exit()

found = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print("Changed: " + cur_dir + "/" + prepare_result)
        found = True

if found == False:
    print("No local git repo changes found in: " + cur_dir)   

```

### Вывод скрипта при запуске при тестировании:
```
user@wbn04pavlov:~/tmp$ ./file2 /home/user
No local git repo changes found in: /home/user
user@wbn04pavlov:~/tmp$ ./file2 /home/userewrg
Path not set or does not exist, using current folder...
/home/user/tmp/file1
user@wbn04pavlov:~/tmp$ ./file2 /home/userewrg
Path not set or does not exist, using current folder...
Changed: /home/user/tmp/file1
user@wbn04pavlov:~/tmp$ ./file2 /home/user/netology
No local git repo changes found in: /home/user/netology
user@wbn04pavlov:~/tmp$ ./file2 /home/user/netology/systemadm-homeworks
Path not set or does not exist, using current folder...
Changed: /home/user/tmp/file1
user@wbn04pavlov:~/tmp$ ./file2 /home/user/netology/sysadm-homeworks
Changed: /home/user/netology/sysadm-homeworks/3
Changed: /home/user/netology/sysadm-homeworks/5
user@wbn04pavlov:~/tmp$ 
```

## Дополнительное задание (со звездочкой*) - необязательно к выполнению

Так получилось, что мы очень часто вносим правки в конфигурацию своей системы прямо на сервере. Но так как вся наша команда разработки держит файлы конфигурации в github и пользуется gitflow, то нам приходится каждый раз переносить архив с нашими изменениями с сервера на наш локальный компьютер, формировать новую ветку, коммитить в неё изменения, создавать pull request (PR) и только после выполнения Merge мы наконец можем официально подтвердить, что новая конфигурация применена. Мы хотим максимально автоматизировать всю цепочку действий. Для этого нам нужно написать скрипт, который будет в директории с локальным репозиторием обращаться по API к github, создавать PR для вливания текущей выбранной ветки в master с сообщением, которое мы вписываем в первый параметр при обращении к py-файлу (сообщение не может быть пустым). При желании, можно добавить к указанному функционалу создание новой ветки, commit и push в неё изменений конфигурации. С директорией локального репозитория можно делать всё, что угодно. Также, принимаем во внимание, что Merge Conflict у нас отсутствуют и их точно не будет при push, как в свою ветку, так и при слиянии в master. Важно получить конечный результат с созданным PR, в котором применяются наши изменения. 

### Ваш скрипт:
```python
???
```

### Вывод скрипта при запуске при тестировании:
```
???
```

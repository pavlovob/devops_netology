# Домашнее задание к занятию "4.3. Языки разметки JSON и YAML"

## Обязательная задача 1
Мы выгрузили JSON, который получили через API запрос к нашему сервису:
```
    { "info" : "Sample JSON output from our service\t",
        "elements" :[
            { "name" : "first",
            "type" : "server",
            "ip" : 7175 
            }
            { "name" : "second",
            "type" : "proxy",
            "ip : 71.78.22.43
            }
        ]
    }
```
  Нужно найти и исправить все ошибки, которые допускает наш сервис
```
{ "info" : "Sample JSON output from our service\t",
    "elements" :[
        { "name" : "first",
        "type" : "server",
        "ip" : 7175 
        },
        { "name" : "second",
        "type" : "proxy",
        "ip" : "71.78.22.43"
        }
    ]
}
```

## Обязательная задача 2
В прошлый рабочий день мы создавали скрипт, позволяющий опрашивать веб-сервисы и получать их IP. К уже реализованному функционалу нам нужно добавить возможность записи JSON и YAML файлов, описывающих наши сервисы. Формат записи JSON по одному сервису: `{ "имя сервиса" : "его IP"}`. Формат записи YAML по одному сервису: `- имя сервиса: его IP`. Если в момент исполнения скрипта меняется IP у сервиса - он должен так же поменяться в yml и json файле.

### Ваш скрипт:
```python
#!/usr/bin/env python3

import os
import socket
import time
import json
import yaml

cur_dir = os.getcwd()
services = ("drive.google.com", "mail.google.com", "google.com")
states = {}

while True:
    for service in services:
        ip = socket.gethostbyname(service)
        print("<" + service + "> - <" + ip + ">")
        time.sleep(1)
        if not states.get(service) is None and states.get(service) != ip:
                print ("[ERROR] <" + service + "> IP mismatch: <" + states[service] + "><" + ip + ">")
        states[service] = ip
        with open("file.json","w") as f:
            json.dump(states,f, indent=2)
        with open("file.yaml","w") as f:
            yaml.dump(states, f, explicit_start=True,explicit_end=True)       
```

### Вывод скрипта при запуске при тестировании:
```
user@wbn04pavlov:~/tmp$ ./file4
<drive.google.com> - <173.194.222.194>
<mail.google.com> - <216.58.209.165>
<google.com> - <216.58.210.174>
<drive.google.com> - <173.194.222.194>
<mail.google.com> - <216.58.209.165>
<google.com> - <216.58.210.174>
<drive.google.com> - <173.194.222.194>
<mail.google.com> - <216.58.209.165>
<google.com> - <216.58.210.174>
<drive.google.com> - <173.194.222.194>
<mail.google.com> - <216.58.210.133>
[ERROR] <mail.google.com> IP mismatch: <216.58.209.165><216.58.210.133>
<google.com> - <216.58.210.142>
[ERROR] <google.com> IP mismatch: <216.58.210.174><216.58.210.142>
<drive.google.com> - <173.194.222.194>
<mail.google.com> - <216.58.210.133>
<google.com> - <216.58.210.142>
<drive.google.com> - <173.194.222.194>
^CTraceback (most recent call last):
  File "./file4", line 17, in <module>
    time.sleep(1)
KeyboardInterrupt
```

### json-файл(ы), который(е) записал ваш скрипт:
```json
{
  "drive.google.com": "173.194.222.194",
  "mail.google.com": "216.58.210.133",
  "google.com": "216.58.210.142"
}
```

### yml-файл(ы), который(е) записал ваш скрипт:
```yaml
---
drive.google.com: 173.194.222.194
google.com: 216.58.210.142
mail.google.com: 216.58.210.133
...
```

## Дополнительное задание (со звездочкой*) - необязательно к выполнению

Так как команды в нашей компании никак не могут прийти к единому мнению о том, какой формат разметки данных использовать: JSON или YAML, нам нужно реализовать парсер из одного формата в другой. Он должен уметь:
   * Принимать на вход имя файла
   * Проверять формат исходного файла. Если файл не json или yml - скрипт должен остановить свою работу
   * Распознавать какой формат данных в файле. Считается, что файлы *.json и *.yml могут быть перепутаны
   * Перекодировать данные из исходного формата во второй доступный (из JSON в YAML, из YAML в JSON)
   * При обнаружении ошибки в исходном файле - указать в стандартном выводе строку с ошибкой синтаксиса и её номер
   * Полученный файл должен иметь имя исходного файла, разница в наименовании обеспечивается разницей расширения файлов

### Ваш скрипт:
```python
???
```

### Пример работы скрипта:
???

## Задача 1

Используя docker поднимите инстанс MySQL (версию 8). Данные БД сохраните в volume.
```
docker run -d --name=mysql  -e MYSQL_ROOT_PASSWORD=mysql -v /home/user/mysql:/var/lib/mysql mysql:8.0
```
Изучите [бэкап БД](https://github.com/netology-code/virt-homeworks/tree/master/06-db-03-mysql/test_data) и 
восстановитесь из него.  
- дамп выложен в volume  
```
mysql> create database test_db;
```
![image](https://user-images.githubusercontent.com/22905019/159044762-2f8f4e53-fb53-4dbe-9c78-5229b724b81a.png)  
Перейдите в управляющую консоль `mysql` внутри контейнера.
Используя команду `\h` получите список управляющих команд.
Найдите команду для выдачи статуса БД и **приведите в ответе** из ее вывода версию сервера БД.  
![image](https://user-images.githubusercontent.com/22905019/159047775-ec702f00-d60f-4e9c-9763-2020ac4d5dd3.png)  
Подключитесь к восстановленной БД и получите список таблиц из этой БД.  
![image](https://user-images.githubusercontent.com/22905019/159048030-2b6cb969-a37b-41e3-af5b-f065144b6d1d.png)  
**Приведите в ответе** количество записей с `price` > 300.  
![image](https://user-images.githubusercontent.com/22905019/159048328-91714034-7948-4e77-8471-b1392dd21318.png)  
## Задача 2

Создайте пользователя test в БД c паролем test-pass, используя:
- плагин авторизации mysql_native_password
- срок истечения пароля - 180 дней 
- количество попыток авторизации - 3 
- максимальное количество запросов в час - 100
- аттрибуты пользователя:
    - Фамилия "Pretty"
    - Имя "James"
```
CREATE USER test IDENTIFIED WITH mysql_native_password BY 'test-pass' WITH MAX_QUERIES_PER_HOUR 100 PASSWORD EXPIRE INTERVAL 180 DAY FAILED_LOGIN_ATTEMPTS 3 ATTRIBUTE '{"Surname":"Pretty",:"Name":"James"}';
```
Предоставьте привелегии пользователю `test` на операции SELECT базы `test_db`.  
```
GRANT 
```
Используя таблицу INFORMATION_SCHEMA.USER_ATTRIBUTES получите данные по пользователю `test` и 
**приведите в ответе к задаче**.

## Задача 3

Установите профилирование `SET profiling = 1`.
Изучите вывод профилирования команд `SHOW PROFILES;`.

Исследуйте, какой `engine` используется в таблице БД `test_db` и **приведите в ответе**.

Измените `engine` и **приведите время выполнения и запрос на изменения из профайлера в ответе**:
- на `MyISAM`
- на `InnoDB`

## Задача 4 

Изучите файл `my.cnf` в директории /etc/mysql.

Измените его согласно ТЗ (движок InnoDB):
- Скорость IO важнее сохранности данных
- Нужна компрессия таблиц для экономии места на диске
- Размер буффера с незакомиченными транзакциями 1 Мб
- Буффер кеширования 30% от ОЗУ
- Размер файла логов операций 100 Мб

Приведите в ответе измененный файл `my.cnf`.

---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---

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
GRANT SELECT ON test_db.* to test; 
```
Используя таблицу INFORMATION_SCHEMA.USER_ATTRIBUTES получите данные по пользователю `test`  
```
select * from INFORMATION_SCHEMA.USER_ATTRIBUTES where USER='test';
```
![image](https://user-images.githubusercontent.com/22905019/159237574-3e2fd120-157d-4741-b2ad-1d70cea38260.png)  

## Задача 3
Установите профилирование `SET profiling = 1`.
Изучите вывод профилирования команд `SHOW PROFILES;`.
Исследуйте, какой `engine` используется в таблице БД `test_db`  
```
select table_name, engine from information_schema.tables where table_schema = 'test_db';
```
![image](https://user-images.githubusercontent.com/22905019/159266401-2312c035-4d15-4fa3-942a-8ee7f199fa46.png)  

Измените `engine` и **приведите время выполнения и запрос на изменения из профайлера в ответе**:
- на `MyISAM`
- на `InnoDB`
![image](https://user-images.githubusercontent.com/22905019/159266795-3a91c5b9-7467-4976-bb8d-7dcfc65b2e69.png)  

## Задача 4 

Изучите файл `my.cnf` в директории /etc/mysql.

Измените его согласно ТЗ (движок InnoDB):
Файл my.cnf:  
```
[mysqld]
pid-file        = /var/run/mysqld/mysqld.pid
socket          = /var/run/mysqld/mysqld.sock
datadir         = /var/lib/mysql
secure-file-priv= NULL

# Custom config should go here
!includedir /etc/mysql/conf.d/

innodb_flush_log_at_trx_commit = 0 
innodb_file_format=Barracuda
innodb_log_buffer_size	= 1M
key_buffer_size = 1229М
max_binlog_size	= 100M
```
---

# Домашнее задание к занятию "6.2. SQL"
## Задача 1
Используя docker поднимите инстанс PostgreSQL (версию 12) c 2 volume, 
в который будут складываться данные БД и бэкапы.
Приведите получившуюся команду или docker-compose манифест.
```
user@user-pc:~$ docker run --name=postgres1  -e POSTGRES_PASSWORD=postgres -v /home/user/postgersdb:/var/lib/postgresql/data -v /home/user/postgresbck:/var/lib/postgresql/backups -d postgres:12
```

## Задача 2
В БД из задачи 1: 
- создайте пользователя test-admin-user и БД test_db
```
CREATE ROLE "test-admin-user" SUPERUSER LOGIN;
CREATE DATABASE test_db;
c\ test_db;
```
- в БД test_db создайте таблицу orders и clients (спeцификация таблиц ниже)
```
CREATE TABLE orders(
    id integer PRIMARY KEY,
    name text,
    price integer);
```
```
CREATE TABLE clients(
    id integer PRIMARY KEY,
    fio text,
    country text,
    order_id integer REFERENCES orders (id)); 
```
- предоставьте привилегии на все операции пользователю test-admin-user на таблицы БД test_db
```
GRANT ALL PRIVILEGES ON DATABASE test_db TO "test-admin-user";
```
хотя этого можно было не делать, т.к. пользователь SUPERUSER
- создайте пользователя test-simple-user  
```
CREATE ROLE "test-admin-user" LOGIN;
```
- предоставьте пользователю test-simple-user права на SELECT/INSERT/UPDATE/DELETE данных таблиц БД test_db  
```
GRANT SELECT, INSERT, UPDATE, DELETE ON orders TO "test-simple-user";
GRANT SELECT, INSERT, UPDATE, DELETE ON clients TO "test-simple-user";
```
Таблица orders:
- id (serial primary key)
- наименование (string)
- цена (integer)

Таблица clients:
- id (serial primary key)
- фамилия (string)
- страна проживания (string, index)
- заказ (foreign key orders)

Приведите:
- итоговый список БД после выполнения пунктов выше,  
![image](https://user-images.githubusercontent.com/22905019/157863133-35362a31-7554-49de-98f6-73c79a467cc8.png)  
- описание таблиц (describe)  
```
\c test_db;
\d orders;
```
![image](https://user-images.githubusercontent.com/22905019/157864576-2097d741-c333-4c70-a2d3-480573e332e2.png)  

```
\d clients;
```
![image](https://user-images.githubusercontent.com/22905019/157864405-44c9ff57-a0ea-49fa-9e8a-caa5cab3447c.png)  
- SQL-запрос для выдачи списка пользователей с правами над таблицами test_db  
```
select * from information_schema.role_table_grants where grantee like '%test%';
```
- список пользователей с правами над таблицами test_db  
![image](https://user-images.githubusercontent.com/22905019/157880627-ab6ec9db-2efc-493d-a1b5-d550bf6e8552.png)  

## Задача 3

Используя SQL синтаксис - наполните таблицы следующими тестовыми данными:  
Таблица orders

|Наименование|цена|
|------------|----|
|Шоколад| 10 |
|Принтер| 3000 |
|Книга| 500 |
|Монитор| 7000|
|Гитара| 4000|  
```
INSERT INTO orders (id,name, price) VALUES(1,'Шоколад',10),(2,'Принтер',3000),(3,'Книга',500),(4,'Монитор',7000),(5,'Гитара',4000);  
```
![image](https://user-images.githubusercontent.com/22905019/157881967-3a96dce6-d505-44b8-9928-af8d6ad1cf8e.png)  

Таблица clients

|ФИО|Страна проживания|
|------------|----|
|Иванов Иван Иванович| USA |
|Петров Петр Петрович| Canada |
|Иоганн Себастьян Бах| Japan |
|Ронни Джеймс Дио| Russia|
|Ritchie Blackmore| Russia|
```
INSERT INTO clients VALUES (1, 'Иванов Иван Иванович', 'USA'), (2, 'Петров Петр Петрович', 'Canada'), (3, 'Иоганн Себастьян Бах', 'Japan'), (4, 'Ронни Джеймс Дио', 'Russia'), (5, 'Ritchie Blackmore', 'Russia');
```
Используя SQL синтаксис вычислите количество записей для каждой таблицы  
![image](https://user-images.githubusercontent.com/22905019/158138864-e6666784-f409-42f1-8d68-d87ea7a68f98.png)  

## Задача 4

Часть пользователей из таблицы clients решили оформить заказы из таблицы orders.  

Используя foreign keys свяжите записи из таблиц, согласно таблице:  

|ФИО|Заказ|
|------------|----|
|Иванов Иван Иванович| Книга |
|Петров Петр Петрович| Монитор |
|Иоганн Себастьян Бах| Гитара |

Приведите SQL-запросы для выполнения данных операций.  
```
UPDATE clients SET order_id = (SELECT id FROM orders WHERE name = 'Книга') WHERE fio = 'Иванов Иван Иванович';
UPDATE clients SET order_id = (SELECT id FROM orders WHERE name = 'Монитор') WHERE fio = 'Петров Петр Петрович';
UPDATE clients SET order_id = (SELECT id FROM orders WHERE name = 'Гитара') WHERE fio = 'Иоганн Себастьян Бах';
```
Приведите SQL-запрос для выдачи всех пользователей, которые совершили заказ, а также вывод данного запроса.  
![image](https://user-images.githubusercontent.com/22905019/158140250-29c4cb4a-7aa5-4b95-86fa-df10a533f964.png)  
## Задача 5

Получите полную информацию по выполнению запроса выдачи всех пользователей из задачи 4 
(используя директиву EXPLAIN).  
![image](https://user-images.githubusercontent.com/22905019/158140799-853243c9-1086-4bbd-9ffe-c350c31ca6ee.png)  
Приведите получившийся результат и объясните что значат полученные значения.  
- Используется последовательный метод чтения;
- Затраты на получение первой строки запроса: 0.00 у.е.;
- Затраты на получение всей выборки запроса: 18.10 у.е.;
- Кол-во возвращаемых строк: 810;
- Кол-во байт в строке запроса: 72;
## Задача 6

Создайте бэкап БД test_db и поместите его в volume, предназначенный для бэкапов (см. Задачу 1).  
```
pg_dump -U 'test-admin-user' -d test_db > /var/lib/postgresql/backups/backup.bck
```
Остановите контейнер с PostgreSQL (но не удаляйте volumes).  
![image](https://user-images.githubusercontent.com/22905019/158145214-f4208f61-c40b-4431-ae0b-babec33c477d.png)  
Поднимите новый пустой контейнер с PostgreSQL.  
```
docker run -d --name=postgres2  -e POSTGRES_PASSWORD=postgres  -v /home/user/postgresbck:/var/lib/postgresql/backups postgres:12
```
![image](https://user-images.githubusercontent.com/22905019/158148043-386ae630-0de3-4479-9047-865cb3c76e64.png)  

Восстановите БД test_db в новом контейнере.  
~~~
user@user-pc:~$ docker exec -it postgres2 bash
root@73d9f85496d7:/# psql -Upostgres
postgres=# CREATE DATABASE test_db;
~~~
![image](https://user-images.githubusercontent.com/22905019/158149344-79060050-963b-4730-905b-350c0e0a6e4b.png)  
```
root@73d9f85496d7:/# psql -Upostgres test_db < /var/lib/postgresql/backups/backup.bck
```
![image](https://user-images.githubusercontent.com/22905019/158149887-95c715a4-0fb7-46d3-a0e1-e070a8d73a1d.png)  

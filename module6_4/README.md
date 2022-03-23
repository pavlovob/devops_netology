## Задача 1
Используя docker поднимите инстанс PostgreSQL (версию 13). Данные БД сохраните в volume.  
```
docker run --name=postgres  -e POSTGRES_PASSWORD=postgres -v /home/user/postgres/db:/var/lib/postgresql/data -d postgres:12
```
Подключитесь к БД PostgreSQL используя `psql`.  
![image](https://user-images.githubusercontent.com/22905019/159694625-51c6aded-ec3c-485e-9c5b-2e707dc6559c.png)  

Воспользуйтесь командой `\?` для вывода подсказки по имеющимся в `psql` управляющим командам.

**Найдите и приведите** управляющие команды для:
- вывода списка БД
```
\l
```
- подключения к БД
```
\c
```
- вывода списка таблиц
```
\dt
```
- вывода описания содержимого таблиц
```
\dd
```
- выхода из psql
```
\q
```
## Задача 2

Используя `psql` создайте БД `test_database`.  
![image](https://user-images.githubusercontent.com/22905019/159703354-c806aef5-0713-4e69-b2ed-dfbd4644e7c1.png)  
Изучите [бэкап БД](https://github.com/netology-code/virt-homeworks/tree/master/06-db-04-postgresql/test_data).

Восстановите бэкап БД в `test_database`.  
![image](https://user-images.githubusercontent.com/22905019/159705201-5ba5da9a-edbf-4da0-adef-a9274627bbb7.png)  
Перейдите в управляющую консоль `psql` внутри контейнера.  
```
psql -U postgres
```
Подключитесь к восстановленной БД и проведите операцию ANALYZE для сбора статистики по таблице.
![image](https://user-images.githubusercontent.com/22905019/159705562-983fbc21-ba7a-4892-9d5a-173e2e6b390c.png)  
Используя таблицу [pg_stats](https://postgrespro.ru/docs/postgresql/12/view-pg-stats), найдите столбец таблицы `orders` 
с наибольшим средним значением размера элементов в байтах.  
![image](https://user-images.githubusercontent.com/22905019/159708380-50233681-169a-4a08-a72e-a7539384830a.png)  

## Задача 3

Архитектор и администратор БД выяснили, что ваша таблица orders разрослась до невиданных размеров и
поиск по ней занимает долгое время. Вам, как успешному выпускнику курсов DevOps в нетологии предложили
провести разбиение таблицы на 2 (шардировать на orders_1 - price>499 и orders_2 - price<=499).

Предложите SQL-транзакцию для проведения данной операции.

Можно ли было изначально исключить "ручное" разбиение при проектировании таблицы orders?

## Задача 4

Используя утилиту `pg_dump` создайте бекап БД `test_database`.

Как бы вы доработали бэкап-файл, чтобы добавить уникальность значения столбца `title` для таблиц `test_database`?

---

### Как cдавать задание

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---

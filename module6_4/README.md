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
\dS
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
Создал SQL скрипт:  
```
ALTER TABLE IF EXISTS public.orders RENAME TO orders_old;
CREATE TABLE IF NOT EXISTS public.orders
(
    id integer NOT NULL DEFAULT nextval('orders_id_seq'::regclass),
    title character varying(80) COLLATE pg_catalog."default" NOT NULL,
    price integer DEFAULT 0
) partition by range (price);
CREATE TABLE IF NOT EXISTS public.orders_1 PARTITION OF orders FOR VALUES FROM (500) TO (2147483647);
CREATE TABLE IF NOT EXISTS public.orders_2 PARTITION OF orders FOR VALUES FROM (0) TO (500);
insert into orders (id, title, price) select * from orders_old;
```
Запуск и результаты:
![image](https://user-images.githubusercontent.com/22905019/159880441-829e5e55-85f1-4613-b8b4-5ef86f014ecf.png)  
![image](https://user-images.githubusercontent.com/22905019/159881596-07fd5c7c-a4d2-4289-b31e-ef097a034a03.png)  

Можно ли было изначально исключить "ручное" разбиение при проектировании таблицы orders?  
- Можно, только в определении таблице Orders тогда нужно было включить переметр "...partition by range (price);"  
## Задача 4

Используя утилиту `pg_dump` создайте бекап БД `test_database`.

Как бы вы доработали бэкап-файл, чтобы добавить уникальность значения столбца `title` для таблиц `test_database`?

---

### Как cдавать задание

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---

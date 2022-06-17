# Домашнее задание к занятию "6.4. PostgreSQL"

### Задача 1
Используя docker поднимите инстанс PostgreSQL (версию 13). Данные БД сохраните в volume.

	docker pull postgres:13
	docker volume create vol_postgres
	docker run --rm --name pg-docker -e POSTGRES_PASSWORD=postgres -ti -p 5432:5432 -v vol_postgres:/var/lib/postgresql/data postgres:13

Подключитесь к БД PostgreSQL используя psql.

	bil@LAPTOP-GJ376PE7:~$ docker exec -it pg-docker bash
	root@ee0ddcbbc88b:/# su postgres
	postgres@ee0ddcbbc88b:/$ psql
	psql (13.7 (Debian 13.7-1.pgdg110+1))
	Type "help" for help.
	postgres=#
	
	или
	
	root@ee0ddcbbc88b:/# psql -h localhost -p 5432 -U postgres
	psql (13.7 (Debian 13.7-1.pgdg110+1))
	Type "help" for help.
	postgres=#
	
Воспользуйтесь командой \? для вывода подсказки по имеющимся в psql управляющим командам.

Найдите и приведите управляющие команды для:
вывода списка БД

	postgres=# \l
									List of databases
	Name    |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges
	-----------+----------+----------+------------+------------+-----------------------
	postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 |
	template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
			|          |          |            |            | postgres=CTc/postgres
	template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
			|          |          |            |            | postgres=CTc/postgres
	(3 rows)
	
	Для более детального вывода \l+

подключения к БД

	\c {DBNAME} 
	или 
	\connect {DBNAME}

вывода списка таблиц

	\d 
	или 
	\dt

вывода описания содержимого таблиц

	\d+ {TABLENAME}
	
выхода из psql

	\q

### Задача 2
Используя psql создайте БД test_database.

	create DATABASE test_database;

Изучите бэкап БД.
Восстановите бэкап БД в test_database.
Перейдите в управляющую консоль psql внутри контейнера.
Подключитесь к восстановленной БД и проведите операцию ANALYZE для сбора статистики по таблице.

	test_database=# ANALYZE VERBOSE public.orders;
	INFO:  analyzing "public.orders"
	INFO:  "orders": scanned 1 of 1 pages, containing 8 live rows and 0 dead rows; 8 rows in sample, 8 estimated total rows
	ANALYZE
	test_database=#

Используя таблицу pg_stats, найдите столбец таблицы orders с наибольшим средним значением размера элементов в байтах.

	test_database=# SELECT tablename,attname,avg_width
	test_database-# FROM "pg_stats"
	test_database-# WHERE
	test_database-# avg_width = (SELECT MAX(avg_width) FROM "pg_stats" WHERE tablename='orders');
	tablename | attname | avg_width
	-----------+---------+-----------
	orders    | title   |        16
	(1 row)

Приведите в ответе команду, которую вы использовали для вычисления и полученный результат.


### Задача 3
Архитектор и администратор БД выяснили, что ваша таблица orders разрослась до невиданных размеров и поиск по ней занимает долгое время. Вам, как успешному выпускнику курсов DevOps в нетологии предложили провести разбиение таблицы на 2 (шардировать на orders_1 - price>499 и orders_2 - price<=499).

Предложите SQL-транзакцию для проведения данной операции.
Можно ли было изначально исключить "ручное" разбиение при проектировании таблицы orders?

	CREATE TABLE public.orders_new (LIKE public.orders INCLUDING ALL EXCLUDING INDEXES) PARTITION BY RANGE (price);
	CREATE TABLE public.orders_new_lower499 PARTITION OF public.orders_new FOR VALUES FROM (MINVALUE) TO (499);
	CREATE TABLE public.orders_new_higher499 PARTITION OF public.orders_new FOR VALUES FROM (499) TO (MAXVALUE);
	CREATE INDEX ON public.orders_new_lower499 (price);
	CREATE INDEX ON public.orders_new_higher499 (price);
	START TRANSACTION;
	INSERT INTO public.orders_new SELECT * FROM public.orders;
	COMMIT;

	Исключить "ручное" разбиение при проектировании таблицы orders на этапе проектирования таблицы возможно, если бы эта таблицасоздавалась как изначально секционированная (оператор PARTITION BY при создании)

### Задача 4
Используя утилиту pg_dump создайте бекап БД test_database.

	postgres@ee0ddcbbc88b:/$ pg_dump -U postgres test_database > /var/lib/postgresql/data/test_database_backup.sql

Как бы вы доработали бэкап-файл, чтобы добавить уникальность значения столбца title для таблиц test_database?

	Создать индекс по нужному полю или констрейн
	CREATE INDEX ON orders ((lower(title)));
	ALTER TABLE ONLY public.orders ADD CONSTRAINT orders_utitle UNIQUE(title);


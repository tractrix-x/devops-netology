# Домашнее задание к занятию "6.2. SQL"
Введение
Перед выполнением задания вы можете ознакомиться с дополнительными материалами.

### Задача 1
Используя docker поднимите инстанс PostgreSQL (версию 12) c 2 volume, в который будут складываться данные БД и бэкапы.
Приведите получившуюся команду или docker-compose манифест.

	docker pull postgres:12
	docker volume create vol1
	docker volume create vol2
	docker run --rm --name pg-docker -e POSTGRES_PASSWORD=postgres -ti -p 5432:5432 -v vol1:/var/lib/postgresql/data -v vol2:/var/lib/postgresql postgres:12

	docker exec -it pg-docker bash
	su postgres
	psql
	\l

![dz6-2_1](/dz_db_6_2/dz6_2-1.jpg) 


### Задача 2
В БД из задачи 1:

создайте пользователя test-admin-user и БД test_db

	postgres=# create database test_db
	postgres-# ;
	CREATE DATABASE
	postgres=#

	просмотр пользователей 
	select * from pg_user;
	
	создание пользователя
	postgres=# CREATE USER test_admin_user WITH PASSWORD 'test';
	CREATE ROLE
	postgres=#
	
	выдаем полномочия на БД
	GRANT ALL PRIVILEGES ON DATABASE "test_db" to test_admin_user;
	
	postgres=# GRANT ALL PRIVILEGES ON DATABASE "test_db" to test_admin_user;
	GRANT
	postgres=#
	
в БД test_db создайте таблицу orders и clients (спeцификация таблиц ниже)

	CREATE TABLE orders 
	(
	id integer, 
	name text, 
	price integer, 
	PRIMARY KEY (id) 
	);
	
	CREATE TABLE clients 
	(
		id integer PRIMARY KEY,
		lastname text,
		country text,
		tovar integer,
		FOREIGN KEY (tovar) REFERENCES orders (Id)
	);

предоставьте привилегии на все операции пользователю test-admin-user на таблицы БД test_db

	подключаемся к БД test_db
	postgres=# \c test_db
	You are now connected to database "test_db" as user "postgres".

	выдаем полномочия на таблицы
	test_db=# GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO "test_admin_user";
	GRANT
	test_db=#

создайте пользователя test-simple-user

	test_db=# CREATE USER test_simple_user WITH PASSWORD 'test';

предоставьте пользователю test-simple-user права на SELECT/INSERT/UPDATE/DELETE данных таблиц БД test_db

	GRANT SELECT ON TABLE public.clients TO "test_simple_user";
	GRANT INSERT ON TABLE public.clients TO "test_simple_user";
	GRANT UPDATE ON TABLE public.clients TO "test_simple_user";
	GRANT DELETE ON TABLE public.clients TO "test_simple_user";
	GRANT SELECT ON TABLE public.orders TO "test_simple_user";
	GRANT INSERT ON TABLE public.orders TO "test_simple_user";
	GRANT UPDATE ON TABLE public.orders TO "test_simple_user";
	GRANT DELETE ON TABLE public.orders TO "test_simple_user";


Таблица orders:

id (serial primary key)
наименование (string)
цена (integer)
Таблица clients:

id (serial primary key)
фамилия (string)
страна проживания (string, index)
заказ (foreign key orders)
Приведите:

итоговый список БД после выполнения пунктов выше,описание таблиц (describe)
SQL-запрос для выдачи списка пользователей с правами над таблицами test_db

Список БД
![dz6-2_2](/dz_db_6_2/dz6_2-2.jpg) 

	\l
	\dt
	SELECT grantee,table_name,privilege_type FROM information_schema.role_table_grants WHERE grantee in ('test_simple_user' ,'test_admin_user');


### Задача 3
список пользователей с правами над таблицами test_db
Используя SQL синтаксис - наполните таблицы следующими тестовыми данными:

	insert into orders VALUES (1, 'Шоколад', 10), (2, 'Принтер', 3000), (3, 'Книга', 500), (4, 'Монитор', 7000), (5, 'Гитара', 4000);
	insert into clients VALUES (1, 'Иванов Иван Иванович', 'USA'), (2, 'Петров Петр Петрович', 'Canada'), (3, 'Иоганн Себастьян Бах', 'Japan'), (4, 'Ронни Джеймс Дио', 'Russia'), (5, 'Ritchie Blackmore', 'Russia');
	select count (*) from orders;
	select count (*) from clients;

вычислите количество записей для каждой таблицы
приведите в ответе:
запросы
результаты их выполнения.

![dz6-2_3](/dz_db_6_2/dz6_2-3.jpg) 

### Задача 4
Часть пользователей из таблицы clients решили оформить заказы из таблицы orders.

Используя foreign keys свяжите записи из таблиц, согласно таблице.Приведите SQL-запросы для выполнения данных операций.

	test_db-# update  clients set tovar = 5 where id = 3;
	test_db=# update  clients set tovar = 3 where id = 1;
	test_db=# update  clients set tovar = 4 where id = 2;
	select c.lastname, o.name from clients c, orders o where c.tovar = o.id

Показывает стоимость выполнения запроса и его подшагов, поля по которым идет сканирование.

![dz6-2_4](/dz_db_6_2/dz6_2-4.jpg) 

Приведите SQL-запрос для выдачи всех пользователей, которые совершили заказ, а также вывод данного запроса.

Подсказк - используйте директиву UPDATE.


### Задача 5
Получите полную информацию по выполнению запроса выдачи всех пользователей из задачи 4 (используя директиву EXPLAIN).

Приведите получившийся результат и объясните что значат полученные значения.

![dz6-2_5](/dz_db_6_2/dz6_2-5.jpg) 


### Задача 6
Создайте бэкап БД test_db и поместите его в volume, предназначенный для бэкапов (см. Задачу 1).

	postgres@0d1ea0195f27:/$ pg_dump -U postgres test_db -f /var/lib/postgresql/data/dump_test.sql
	postgres@0d1ea0195f27:/$ ls /var/lib/postgresql/data/dump_test.sql
	/var/lib/postgresql/data/dump_test.sql
	postgres@0d1ea0195f27:/$

Остановите контейнер с PostgreSQL (но не удаляйте volumes).

	docker stop pg-docker

Поднимите новый пустой контейнер с PostgreSQL.

	docker run --rm --name pg-docker2 -e POSTGRES_PASSWORD=postgres -ti -p 5432:5432 -v vol1:/var/lib/postgresql/data -v vol2:/var/lib/postgresql postgres:12

Восстановите БД test_db в новом контейнере.

	docker exec -it pg-docker2 bash
	postgres@1ed87e96d2f0:/$ psql
	postgres=# drop database test_db;
	postgres=# create database test_db2
	восстановление
	postgres@1ed87e96d2f0:/$ psql -U postgres -d test_db2 -f /var/lib/postgresql/data/dump_test.sql

Приведите список операций, который вы применяли для бэкапа данных и восстановления.

![dz6-2_6](/dz_db_6_2/dz6_2-6.jpg) 

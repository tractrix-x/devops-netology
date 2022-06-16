# Домашнее задание к занятию "6.3. MySQL"
Введение
Перед выполнением задания вы можете ознакомиться с дополнительными материалами.

### Задача 1
Используя docker поднимите инстанс MySQL (версию 8). Данные БД сохраните в volume.

Изучите бэкап БД и восстановитесь из него.

Перейдите в управляющую консоль mysql внутри контейнера.

	docker pull mysql:8.0
	docker volume create vol_mysql
	docker exec -it mysql-docker bash
	mysql -uroot -p mysql

Используя команду \h получите список управляющих команд.

![dz6-3_1-0](/dz_db_6_3/6_3_1-0.jpg)

Найдите команду для выдачи статуса БД и приведите в ответе из ее вывода версию сервера БД.

![dz6-3_1-1](/dz_db_6_3/6_3_1-1.jpg)	

Подключитесь к восстановленной БД и получите список таблиц из этой БД.

	SHOW DATABASES;
	![dz6-3_1-2](/dz_db_6_3/6_3_1-2.jpg)	

Приведите в ответе количество записей с price > 300.

	SELECT COUNT(*) FROM orders WHERE price > 300;

![dz6-3_1-3](/dz_db_6_3/6_3_1-3.jpg)	

В следующих заданиях мы будем продолжать работу с данным контейнером.


### Задача 2
Создайте пользователя test в БД c паролем test-pass, используя:

плагин авторизации mysql_native_password
срок истечения пароля - 180 дней
количество попыток авторизации - 3
максимальное количество запросов в час - 100
аттрибуты пользователя:
Фамилия "Pretty"
Имя "James"
Предоставьте привелегии пользователю test на операции SELECT базы test_db.

Используя таблицу INFORMATION_SCHEMA.USER_ATTRIBUTES получите данные по пользователю test и приведите в ответе к задаче.

	CREATE USER 'test'@'localhost' IDENTIFIED BY 'mysql';
	ALTER USER 'test'@'localhost' ATTRIBUTE '{"fname":"James", "lname":"Pretty"}';
	ALTER USER 'test'@'localhost' 
		IDENTIFIED BY 'test-pass' 
		WITH
		MAX_QUERIES_PER_HOUR 100
		PASSWORD EXPIRE INTERVAL 180 DAY
		FAILED_LOGIN_ATTEMPTS 3 PASSWORD_LOCK_TIME 2;
	GRANT Select ON test_db.orders TO 'test'@'localhost';
	
	SELECT * FROM INFORMATION_SCHEMA.USER_ATTRIBUTES WHERE USER='test';
	
![dz6-3_2-1](/dz_db_6_3/6_3_2-1.jpg)	
	

### Задача 3
Установите профилирование SET profiling = 1. Изучите вывод профилирования команд SHOW PROFILES;.

	SET profiling = 1
	
![dz6-3_3-1](/dz_db_6_3/6_3_3-1.jpg)	

Исследуйте, какой engine используется в таблице БД test_db и приведите в ответе.

	SELECT TABLE_NAME,ENGINE,ROW_FORMAT,TABLE_ROWS,DATA_LENGTH,INDEX_LENGTH FROM information_schema.TABLES 
	WHERE table_name = 'orders' and  TABLE_SCHEMA = 'test_db' ORDER BY ENGINE asc;
	
![dz6-3_3-2](/dz_db_6_3/6_3_3-2.jpg)	

Измените engine и приведите время выполнения и запрос на изменения из профайлера в ответе:

на MyISAM
на InnoDB

	ALTER TABLE orders ENGINE = MyISAM;
	ALTER TABLE orders ENGINE = InnoDB;
	show profiles;
	
![dz6-3_3-3](/dz_db_6_3/6_3_3-3.jpg)	

### Задача 4
Изучите файл my.cnf в директории /etc/mysql.

Измените его согласно ТЗ (движок InnoDB):

Скорость IO важнее сохранности данных
Нужна компрессия таблиц для экономии места на диске
Размер буффера с незакомиченными транзакциями 1 Мб
Буффер кеширования 30% от ОЗУ
Размер файла логов операций 100 Мб
Приведите в ответе измененный файл my.cnf.

![dz6-3_4-1](/dz_db_6_3/6_3_4-1.jpg)


# Домашнее задание к занятию "5.3. Введение. Экосистема. Архитектура. Жизненный цикл Docker контейнера"
Как сдавать задания
Обязательными к выполнению являются задачи без указания звездочки. Их выполнение необходимо для получения зачета и диплома о профессиональной переподготовке.

Задачи со звездочкой (*) являются дополнительными задачами и/или задачами повышенной сложности. Они не являются обязательными к выполнению, но помогут вам глубже понять тему.

Домашнее задание выполните в файле readme.md в github репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.

Любые вопросы по решению задач задавайте в чате учебной группы.

### Задача 1
Сценарий выполения задачи:

создайте свой репозиторий на https://hub.docker.com;
выберете любой образ, который содержит веб-сервер Nginx;
создайте свой fork образа;
реализуйте функциональность: запуск веб-сервера в фоне с индекс-страницей, содержащей HTML-код ниже:
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I’m DevOps Engineer!</h1>
</body>
</html>
Опубликуйте созданный форк в своем репозитории и предоставьте ответ в виде ссылки на https://hub.docker.com/username_repo.

	
	cd /home/bil/.docker/mynginx
	bil@LAPTOP-GJ376PE7:~/.docker/mynginx$ ls -a
	cat Dockerfile
	FROM nginx:1.21
	COPY index.html /usr/share/nginx/html/

	Сборка образа:
	docker build -t xtractrix/nginx:v1 .
	
	Публикация образа:
	docker login --username xtractrix
	docker push xtractrix/nginx:v1
	
	ссылка на репозиторий < https://hub.docker.com/repository/docker/xtractrix/nginx/tags?page=1&ordering=last_updated >


### Задача 2
Посмотрите на сценарий ниже и ответьте на вопрос: "Подходит ли в этом сценарии использование Docker контейнеров или лучше подойдет виртуальная машина, физическая машина? Может быть возможны разные варианты?"

Детально опишите и обоснуйте свой выбор.

--

Сценарий:

Высоконагруженное монолитное java веб-приложение;

	Docker не подходит: у JVM нет встроенных средств для корректной работы внутри контейнера с высоконагруженным приложением.

Nodejs веб-приложение;

	Docker подходит: веб приложение не зависит от архитектуры платформы и операционной системы, поэтому может работь в контейнеризированной среде.

Мобильное приложение c версиями для Android и iOS;

	Подойдет ВМ с определенным окружением и ядром для запуска Android и iOS.

Шина данных на базе Apache Kafka;

	Физическая машина 

Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana;

	ВМ т.к. логирование (logstash) использует Java, использововав которую в контейнере для продуктивной системы можно получить проблемы из-за нехватки ресурсов.

Мониторинг-стек на базе Prometheus и Grafana;

	Docker подойдет если хранение метрик вынести за пределы контейнера.

MongoDB, как основное хранилище данных для java-приложения;

	Физическая или виртуальная машина: для постоянного хранения данных stateless-контейнеры не подходят.

Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry.

	Физическая или виртуальная машина учше подойдут: процессы GitLab и GitLab Runner можно запускать в контейнерах, а хранение данных и образов Docker Registry нужно выносить за пределы контейнеров. 

### Задача 3
Запустите первый контейнер из образа centos c любым тэгом в фоновом режиме, подключив папку /data из текущей рабочей директории на хостовой машине в /data контейнера;
Запустите второй контейнер из образа debian в фоновом режиме, подключив папку /data из текущей рабочей директории на хостовой машине в /data контейнера;
Подключитесь к первому контейнеру с помощью docker exec и создайте текстовый файл любого содержания в /data;
Добавьте еще один файл в папку /data на хостовой машине;
Подключитесь во второй контейнер и отобразите листинг и содержание файлов в /data контейнера.

	Запуск centos:centos8.4.2105 и debian:bullseye :
	
	bil@LAPTOP-GJ376PE7:~/.docker/mynginx$ docker run --name centos -v /home/vagrant/data:/data -d -t centos:centos8.4.2105
	Unable to find image 'centos:centos8.4.2105' locally
	centos8.4.2105: Pulling from library/centos
	a1d0c7532777: Pull complete
	Digest: sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177
	Status: Downloaded newer image for centos:centos8.4.2105
	575f5386d0d9f2ec483645a98ce0d279693a042f14483a4b04afb6bfab962fb4
	bil@LAPTOP-GJ376PE7:~/.docker/mynginx$ docker run --name debian -v /home/vagrant/data:/data -d -t debian:bullseye
	Unable to find image 'debian:bullseye' locally
	bullseye: Pulling from library/debian
	e756f3fdd6a3: Pull complete
	Digest: sha256:3f1d6c17773a45c97bd8f158d665c9709d7b29ed7917ac934086ad96f92e4510
	Status: Downloaded newer image for debian:bullseye
	d1256c06ef8829fc8623abede23ab2e1073e907ec3c0cfd000af566adac1d084
	bil@LAPTOP-GJ376PE7:~/.docker/mynginx$ docker ps
	CONTAINER ID   IMAGE                   COMMAND       CREATED              STATUS              PORTS     NAMES
	d1256c06ef88   debian:bullseye         "bash"        12 seconds ago       Up 11 seconds                 debian
	575f5386d0d9   centos:centos8.4.2105   "/bin/bash"   About a minute ago   Up About a minute             centos
	bil@LAPTOP-GJ376PE7:~/.docker/mynginx$

	bil@LAPTOP-GJ376PE7:/opt/docker/mynginx$ docker exec -it centos bash
	[root@575f5386d0d9 /]# pwd
	/
	[root@575f5386d0d9 /]# mkdir /data/file_1.txt
	[root@575f5386d0d9 /]# cd /data
	[root@575f5386d0d9 data]# ls -a
	.  ..  file_1.txt
	
	bil@LAPTOP-GJ376PE7:~/.docker/mynginx$ cd data/
	bil@LAPTOP-GJ376PE7:~/.docker/mynginx/data$ touch file_2.txt
	bil@LAPTOP-GJ376PE7:~/.docker/mynginx/data$ ls -a
	.  ..  file_2.txt
	bil@LAPTOP-GJ376PE7:~/.docker/mynginx/data$

	bil@LAPTOP-GJ376PE7:~/.docker/mynginx/data$ docker exec -it debian bash
	root@d1256c06ef88:/# cd /data
	root@d1256c06ef88:/data# ls
	file_2.txt  first_file.txt
	root@d1256c06ef88:/data#

### Задача 4 (*)
Воспроизвести практическую часть лекции самостоятельно.

Соберите Docker образ с Ansible, загрузите на Docker Hub и пришлите ссылку вместе с остальными ответами к задачам.



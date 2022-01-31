# Домашнее задание к занятию "3.4. Операционные системы, лекция 2"

### 1. На лекции мы познакомились с node_exporter. В демонстрации его исполняемый файл запускался в background. Этого достаточно для демо, но не для настоящей production-системы, где процессы должны находиться под внешним управлением. Используя знания из лекции по systemd, создайте самостоятельно простой unit-файл для node_exporter:

#### Доработка:
> Задание 1
> Предлагаю уточнить как именно в службу будут передаваться дополнительные опции. Примеры можно посмотреть вот здесь:
> https://www.freedesktop.org/software/systemd/man/systemd.service.html#ExecStart=
> https://unix.stackexchange.com/questions/323914/dynamic-variables-in-systemd-service-unit-files
> https://stackoverflow.com/questions/48843949/systemd-use-variables-in-a-unit-file
> Замечу, что речь идёт не о переменных окружения, а об опциях (параметрах) запуска службы.

	Опции запуска службы помещаются в конфигурационный файл (у меня это /etc/systemd/system/node_exporter.service) в секцию[Service]
	
			[Service]
		#ExecStart=/opt/node_exporter/node_exporter
		ExecStart=/home/vagrant/node_exporter-1.3.1.linux-amd64/node_exporter --collector.nfs --collector.nfsd --web.listen-address 0.0.0.0:9100
		WorkingDirectory=/home/vagrant/node_exporter-1.3.1.linux-amd64
		EnvironmentFile=/etc/default/node_exporter
		User=vagrant
		Group=vagrant



Ставим node-exporter по \https://prometheus.io/docs/guides/node-exporter/
В vagrant apt-get угается "Bus errorackage lists... 0%" обновила apt-get 
sudo apt-get update
sudo apt-get install wget

	vagrant@vagrant:~$ sudo apt-get update
	Hit:1 http://archive.ubuntu.com/ubuntu focal InRelease
	Get:2 http://security.ubuntu.com/ubuntu focal-security InRelease [114 kB]
	Get:3 http://archive.ubuntu.com/ubuntu focal-updates InRelease [114 kB]
	Get:4 http://archive.ubuntu.com/ubuntu focal-backports InRelease [108 kB]
	Get:5 http://security.ubuntu.com/ubuntu focal-security/main amd64 Packages [1,178 kB]
	Get:6 http://archive.ubuntu.com/ubuntu focal-updates/main amd64 Packages [1,510 kB]
	Get:7 http://archive.ubuntu.com/ubuntu focal-updates/main i386 Packages [592 kB]
	Get:8 http://security.ubuntu.com/ubuntu focal-security/main i386 Packages [363 kB]
	Get:9 http://archive.ubuntu.com/ubuntu focal-updates/main Translation-en [296 kB]
	Get:10 http://security.ubuntu.com/ubuntu focal-security/main Translation-en [210 kB]
	Get:11 http://archive.ubuntu.com/ubuntu focal-updates/restricted i386 Packages [21.8 kB]
	Get:12 http://archive.ubuntu.com/ubuntu focal-updates/restricted amd64 Packages [736 kB]
	Get:13 http://security.ubuntu.com/ubuntu focal-security/restricted i386 Packages [20.5 kB]
	Get:14 http://security.ubuntu.com/ubuntu focal-security/restricted amd64 Packages [686 kB]
	Get:15 http://archive.ubuntu.com/ubuntu focal-updates/restricted Translation-en [105 kB]
	Get:16 http://archive.ubuntu.com/ubuntu focal-updates/universe amd64 Packages [894 kB]
	Get:17 http://security.ubuntu.com/ubuntu focal-security/restricted Translation-en [97.9 kB]
	Get:18 http://security.ubuntu.com/ubuntu focal-security/universe amd64 Packages [677 kB]
	Get:19 http://archive.ubuntu.com/ubuntu focal-updates/universe i386 Packages [664 kB]
	Get:20 http://archive.ubuntu.com/ubuntu focal-updates/universe Translation-en [196 kB]
	Get:21 http://security.ubuntu.com/ubuntu focal-security/universe i386 Packages [532 kB]
	Get:22 http://archive.ubuntu.com/ubuntu focal-updates/multiverse amd64 Packages [24.8 kB]
	Get:23 http://archive.ubuntu.com/ubuntu focal-updates/multiverse i386 Packages [8,432 B]
	Get:24 http://archive.ubuntu.com/ubuntu focal-updates/multiverse Translation-en [6,928 B]
	Get:25 http://archive.ubuntu.com/ubuntu focal-backports/main i386 Packages [34.5 kB]
	Get:26 http://archive.ubuntu.com/ubuntu focal-backports/main amd64 Packages [42.0 kB]
	Get:27 http://archive.ubuntu.com/ubuntu focal-backports/main Translation-en [10.0 kB]
	Get:28 http://archive.ubuntu.com/ubuntu focal-backports/universe i386 Packages [11.5 kB]
	Get:29 http://archive.ubuntu.com/ubuntu focal-backports/universe amd64 Packages [20.8 kB]
	Get:30 http://archive.ubuntu.com/ubuntu focal-backports/universe Translation-en [14.3 kB]
	Get:31 http://security.ubuntu.com/ubuntu focal-security/universe Translation-en [115 kB]
	Get:32 http://security.ubuntu.com/ubuntu focal-security/multiverse amd64 Packages [21.8 kB]
	Get:33 http://security.ubuntu.com/ubuntu focal-security/multiverse i386 Packages [7,176 B]
	Get:34 http://security.ubuntu.com/ubuntu focal-security/multiverse Translation-en [4,948 B]
	Fetched 9,438 kB in 14s (681 kB/s)
	Reading package lists... Done
	vagrant@vagrant:~$

Потом возникли проблемы со скачиванием архива, поправила /etc/resolv.conf

	root@vagrant:/home/vagrant# cat /etc/resolv.conf
	# This file is managed by man:systemd-resolved(8). Do not edit.
	#
	# This is a dynamic resolv.conf file for connecting local clients to the
	# internal DNS stub resolver of systemd-resolved. This file lists all
	# configured search domains.
	#
	# Run "resolvectl status" to see details about the uplink DNS servers
	# currently in use.
	#
	# Third party programs must not access this file directly, but only through the
	# symlink at /etc/resolv.conf. To manage man:resolv.conf(5) in a different way,
	# replace this symlink by a static file or a different symlink.
	#
	# See man:systemd-resolved.service(8) for details about the supported modes of
	# operation for /etc/resolv.conf.
	nameserver 172.17.128.1
	#nameserver 127.0.0.53
	#options edns0 trust-ad
	#search Home
	root@vagrant:/home/vagrant#

Скачала и растарила

	vagrant@vagrant:~$ wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz
	--2022-01-26 18:07:34--  https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz
	Resolving github.com (github.com)... 140.82.121.3
	Connecting to github.com (github.com)|140.82.121.3|:443... connected.
	HTTP request sent, awaiting response... 302 Found
	Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/9524057/7c60f6f9-7b41-446c-be81-a6c24a9d0383?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20220126%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20220126T182416Z&X-Amz-Expires=300&X-Amz-Signature=2a730ed6882bb5df29948936812963fb6951e1dd191f26a1f51dd2883591e6e0&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=9524057&response-content-disposition=attachment%3B%20filename%3Dnode_exporter-1.3.1.linux-amd64.tar.gz&response-content-type=application%2Foctet-stream [following]
	--2022-01-26 18:07:34--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/9524057/7c60f6f9-7b41-446c-be81-a6c24a9d0383?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20220126%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20220126T182416Z&X-Amz-Expires=300&X-Amz-Signature=2a730ed6882bb5df29948936812963fb6951e1dd191f26a1f51dd2883591e6e0&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=9524057&response-content-disposition=attachment%3B%20filename%3Dnode_exporter-1.3.1.linux-amd64.tar.gz&response-content-type=application%2Foctet-stream
	Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
	Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.108.133|:443... connected.
	HTTP request sent, awaiting response... 200 OK
	Length: 9033415 (8.6M) [application/octet-stream]
	Saving to: ‘node_exporter-1.3.1.linux-amd64.tar.gz’
	
	node_exporter-1.3.1.linux-amd64.tar.gz 100%[============================================================================>]   8.61M  2.60MB/s    in 3.3s
	
	2022-01-26 18:07:38 (2.60 MB/s) - ‘node_exporter-1.3.1.linux-amd64.tar.gz’ saved [9033415/9033415]
	
	vagrant@vagrant:~$
	
	vagrant@vagrant:~$ tar xvfz node_exporter-1.3.1.linux-amd64.tar.gz
	node_exporter-1.3.1.linux-amd64/
	node_exporter-1.3.1.linux-amd64/LICENSE
	node_exporter-1.3.1.linux-amd64/NOTICE
	node_exporter-1.3.1.linux-amd64/node_exporter
	
	vagrant@vagrant:~$ cd node_exporter-1.3.1.linux-amd64/
	vagrant@vagrant:~/node_exporter-1.3.1.linux-amd64$

Создание  unit-файла

	root@vagrant:/home/vagrant# chmod 755 /etc/systemd/system/node_exporter.service
	vagrant@vagrant:~/node_exporter-1.3.1.linux-amd64$ cat /etc/systemd/system/node_exporter.service
	#!/bin/sh -
	# /etc/systemd/system/node_exporter.service/node_exporter.service
	[Unit]
	Description=node_exporter
	
	[Service]
	#ExecStart=/opt/node_exporter/node_exporter
	ExecStart=/home/vagrant/node_exporter-1.3.1.linux-amd64/node_exporter --collector.nfs --collector.nfsd --web.listen-address 0.0.0.0:9100
	WorkingDirectory=/home/vagrant/node_exporter-1.3.1.linux-amd64
	EnvironmentFile=/etc/default/node_exporter
	User=vagrant
	Group=vagrant
	
	[Install]
	Wantedby=multi-user.target
	
	root@vagrant:/home/vagrant# systemctl daemon-reload
	
	root@vagrant:/home/vagrant# systemctl enable node_exporter
	The unit files have no installation config (WantedBy=, RequiredBy=, Also=,
	Alias= settings in the [Install] section, and DefaultInstance= for template
	units). This means they are not meant to be enabled using systemctl.
	Possible reasons for having this kind of units are:
	• A unit may be statically enabled by being symlinked from another unit's
	.wants/ or .requires/ directory.
	• A unit's purpose may be to act as a helper for some other unit which has
	a requirement dependency on it.
	• A unit may be started when needed via activation (socket, path, timer,
	D-Bus, udev, scripted systemctl call, ...).
	• In case of template units, the unit is meant to be enabled with some
	instance name specified.
	
	root@vagrant:/home/vagrant# systemctl daemon-reload

	root@vagrant:/home/vagrant# systemctl -l status node_exporter
	node_exporter.service - node_exporter
	Loaded: loaded (/etc/systemd/system/node_exporter.service; enabled; vendor preset: enabled)
	Active: inactive (dead)
	
Проверка использования переменных окружения процессом node_exporter

	vagrant@vagrant:\~/node_exporter-1.3.1.linux-amd64 sudo cat /proc/1652/environ	LANG=en_US.UTF-8LANGUAGE=en_US:PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/binLANGUAGE=en_US:PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/binHOME=/home/vagrantLOGNAME=vagrantUSER=vagrantSHELL=/bin/bashINVOCATION_ID=0fcb24d52895405c875cbb9cbc28d3ffJOURNAL_STREAM=9:35758MYVAR=some_valueJOURNAL_STREAM=9:30179vagrant@vagrant:~/node_exporter-1.3.1.linux-amd64$
	
Проверка того, что через systemctl процесс корректно стартует, завершается, а после перезагрузки автоматически поднимается.

	vagrant@vagrant:~/node_exporter-1.3.1.linux-amd64$ systemctl stop node_exporter
	==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
	Authentication is required to stop 'node_exporter.service'.
	Authenticating as: vagrant,,, (vagrant)
	Password:
	==== AUTHENTICATION COMPLETE ===
	vagrant@vagrant:~/node_exporter-1.3.1.linux-amd64$ ps -ef | grep node_exporter
	vagrant     1642    1078  0 03:55 pts/1    00:00:00 grep --color=auto node_exporter
	
	vagrant@vagrant:~/node_exporter-1.3.1.linux-amd64$ systemctl start node_exporter
	==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
	Authentication is required to start 'node_exporter.service'.
	Authenticating as: vagrant,,, (vagrant)
	Password:
	==== AUTHENTICATION COMPLETE ===
	vagrant@vagrant:~/node_exporter-1.3.1.linux-amd64$ ps -ef | grep node_exporter
	vagrant     1652       1  0 03:55 ?        00:00:00 /home/vagrant/node_exporter-1.3.1.linux-amd64/node_exporter --collector.nfs --collector.nfsd --web.listen-address 0.0.0.0:9100
	vagrant     1667    1078  0 03:55 pts/1    00:00:00 grep --color=auto node_exporter
	vagrant@vagrant:~/node_exporter-1.3.1.linux-amd64$
	
	vagrant@vagrant:~/node_exporter-1.3.1.linux-amd64$ systemctl restart node_exporter
	==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
	Authentication is required to restart 'node_exporter.service'.
	Authenticating as: vagrant,,, (vagrant)
	Password:
	==== AUTHENTICATION COMPLETE ===
	vagrant@vagrant:~/node_exporter-1.3.1.linux-amd64$ ps -ef | grep node_exporter
	vagrant     1703       1  0 09:27 ?        00:00:00 /home/vagrant/node_exporter-1.3.1.linux-amd64/node_exporter --collector.nfs --collector.nfsd --web.listen-address 0.0.0.0:9100
	vagrant     1721    1078  0 09:27 pts/1    00:00:00 grep --color=auto node_exporter
	vagrant@vagrant:~/node_exporter-1.3.1.linux-amd64$

### 2. Ознакомьтесь с опциями node_exporter и выводом /metrics по-умолчанию. Приведите несколько опций, которые вы бы выбрали для базового мониторинга хоста по CPU, памяти, диску и сети.

	CPU:
	process_cpu_seconds_total
    node_cpu_seconds_total{cpu="0",mode="idle"} 
    node_cpu_seconds_total{cpu="0",mode="system"}
    node_cpu_seconds_total{cpu="0",mode="user"}
    Memory:
    node_memory_MemAvailable_bytes 
    node_memory_MemFree_bytes
	Disk:
    node_disk_io_time_seconds_total{device="sda"} 
    node_disk_read_bytes_total{device="sda"} 
    node_disk_read_time_seconds_total{device="sda"} 
    node_disk_write_time_seconds_total{device="sda"}
	Network:
	node_network_receive_bytes_total{device="eth0"} 
    node_network_transmit_bytes_total{device="eth0"}
    node_network_receive_errs_total{device="eth0"} 
    node_network_transmit_errs_total{device="eth0"}


### 3. Установите в свою виртуальную машину Netdata. Воспользуйтесь готовыми пакетами для установки (sudo apt install -y netdata). После успешной установки: в конфигурационном файле /etc/netdata/netdata.conf в секции [web] замените значение с localhost на bind to = 0.0.0.0, добавьте в Vagrantfile проброс порта Netdata на свой локальный компьютер и сделайте vagrant reload: config.vm.network "forwarded_port", guest: 19999, host: 19999 После успешной перезагрузки в браузере на своем ПК (не в виртуальной машине) вы должны суметь зайти на localhost:19999. Ознакомьтесь с метриками, которые по умолчанию собираются Netdata и с комментариями, которые даны к этим метрикам.

	https://packagecloud.io/netdata/netdata/install#bash-deb
	vagrant@vagrant:~$ vi script.deb.sh
	vagrant@vagrant:~$ curl -s https://packagecloud.io/install/repositories/netdata/netdata/script.deb.sh | sudo bash
	Detected operating system as Ubuntu/focal.
	Checking for curl...
	Detected curl...
	Checking for gpg...
	Detected gpg...
	Running apt-get update... done.
	Installing apt-transport-https... done.
	Installing /etc/apt/sources.list.d/netdata_netdata.list...done.
	Importing packagecloud gpg key... done.
	Running apt-get update... done.
	The repository is setup! You can now install packages.
	
	vagrant@vagrant:~$ sudo apt install -y netdata
	
	vagrant@vagrant:~$ which netdata
	/usr/sbin/netdata
	vagrant@vagrant:~$
	vagrant@vagrant:~$ cat /etc/netdata/netdata.conf | grep bind
		bind to = 0.0.0.0
	vagrant@vagrant:~$
	
	vagrant@vagrant:~$ sudo lsof -i :19999
	COMMAND  PID    USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
	netdata 2490 netdata    5u  IPv6  32686      0t0  TCP localhost:19999 (LISTEN)
	netdata 2490 netdata    6u  IPv4  32687      0t0  TCP localhost:19999 (LISTEN)
	vagrant@vagrant:~$

Пожключиться не смогла.

### 4. Можно ли по выводу dmesg понять, осознает ли ОС, что загружена не на настоящем оборудовании, а на системе виртуализации?
Думаю да, в выводе это есть.

	vagrant@vagrant:~$ dmesg |grep virtualiz
	[    0.007974] CPU MTRRs all blank - virtualized system.
	[    0.054936] Booting paravirtualized kernel on KVM
	[    0.289046] Performance Events: PMU not available due to virtualization, using software events only.
	[    5.920397] systemd[1]: Detected virtualization oracle.
	vagrant@vagrant:~$

	bil@LAPTOP-GJ376PE7:~$ dmesg |grep virtualiz
	[    0.053468] Booting paravirtualized kernel on Hyper-V
	[    0.172148] Performance Events: PMU not available due to virtualization, using software events only.
	bil@LAPTOP-GJ376PE7:~$

### 5. Как настроен sysctl fs.nr_open на системе по-умолчанию? Узнайте, что означает этот параметр. Какой другой существующий лимит не позволит достичь такого числа (ulimit --help)?
fs.nr_open -это системное максимальное число открытых дескрипторов файла, для пользователя задать больше этого числа нельзя, если не менять значение.
Число задается кратное 1024. 
	
	vagrant@vagrant:\~$ /sbin/sysctl -n fs.nr_open
	1048576
	vagrant@vagrant:\~$
	
Или посмотреть в /proc/sys/fs/nr_open

ulimit -a -ограничение, которое может изменяться процессами динамически

	vagrant@vagrant:\~$ ulimit -a
	open files                      (-n) 1024
	
ulimit -aH заданное значение, и может быть изменен только root
	
	vagrant@vagrant:\~$ ulimit -aH
	open files                      (-n) 1048576
	
Максимальное возможное число открытых файлов в системе
	vagrant@vagrant:\~$ cat /proc/sys/fs/file-max
	9223372036854775807
	vagrant@vagrant:\~$
	
### 6. Запустите любой долгоживущий процесс (не ls, который отработает мгновенно, а, например, sleep 1h) в отдельном неймспейсе процессов; покажите, что ваш процесс работает под PID 1 через nsenter. Для простоты работайте в данном задании под root (sudo -i). Под обычным пользователем требуются дополнительные опции (--map-root-user) и т.д.

####Доработка:
> Попробуете выполнить так, чтобы PID был равен 1?

	vagrant@vagrant:~$ sudo -i
	root@vagrant:~# unshare -f --pid --mount-proc sleep 1h
	^Z
	^C
	
	root@vagrant:~# ps -e |grep sleep
		866 pts/0    00:00:00 sleep
		
	root@vagrant:~# ps aux
	root         866  0.0  0.0   8076   596 pts/0    S    17:44   0:00 sleep 1h
	
	root@vagrant:~# nsenter --target 866 --pid --mount
	root@vagrant:/# ps
		PID TTY          TIME CMD
		1 pts/0    00:00:00 sleep
		2 pts/0    00:00:00 bash
		11 pts/0    00:00:00 ps
	root@vagrant:/#

	
### 7. Найдите информацию о том, что такое :(){ :|:& };:. Запустите эту команду в своей виртуальной машине Vagrant с Ubuntu 20.04 (это важно, поведение в других ОС не проверялось). Некоторое время все будет "плохо", после чего (минуты) – ОС должна стабилизироваться. Вызов dmesg расскажет, какой механизм помог автоматической стабилизации. Как настроен этот механизм по-умолчанию, и как изменить число процессов, которое можно создать в сессии?

	https://wiki.merionet.ru/servernye-resheniya/33/10-komand-linux-kotorye-ubyut-vash-server/
	> :(){ :|:& };: - Логическая бомба (известная также как fork bomb), забивающая память системы, что в итоге приводит к её зависанию.
	
	vagrant@vagrant:~$ :(){ :|:& };:
	[1] 1896
	vagrant@vagrant:~$ -bash: fork: retry: Resource temporarily unavailable

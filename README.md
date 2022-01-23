# Домашнее задание к занятию "3.3. Операционные системы, лекция 1"

### 1. Какой системный вызов делает команда cd? В прошлом ДЗ мы выяснили, что cd не является самостоятельной программой, это shell builtin, поэтому запустить strace непосредственно на cd не получится. Тем не менее, вы можете запустить strace на /bin/bash -c 'cd /tmp'. В этом случае вы увидите полный список системных вызовов, которые делает сам bash при старте. Вам нужно найти тот единственный, который относится именно к cd. Обратите внимание, что strace выдаёт результат своей работы в поток stderr, а не в stdout.

	strace /bin/bash -c 'cd /tmp'
	chdir("/tmp")                           = 0

### 2. Попробуйте использовать команду file на объекты разных типов на файловой системе. Например: 
### vagrant@netology1:~\$ file /dev/tty
### /dev/tty: character special (5/0)
### vagrant@netology1:~\$ file /dev/sda
### /dev/sda: block special (8/0)
### vagrant@netology1:~\$ file /bin/bash
### /bin/bash: ELF 64-bit LSB shared object, x86-64
### Используя strace выясните, где находится база данных file на основании которой она делает свои догадки.

Предполагаю, что база данных file находится в /usr/share/misc/magic.mgc, т.к. это бинарный файл.

	vagrant@netology1:~$ file /dev/tty
	/dev/tty: character special (5/0)
	strace file /dev/tty
	openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libmagic.so.1", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/lib/x86_64-linux-gnu/liblzma.so.5", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libbz2.so.1.0", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libz.so.1", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libpthread.so.0", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/usr/lib/locale/locale-archive", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/etc/magic", O_RDONLY) = 3
	openat(AT_FDCWD, "/usr/share/misc/magic.mgc", O_RDONLY) = 3
	openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/gconv/gconv-modules.cache", O_RDONLY) = 3
	
	vagrant@netology1:~$ file /dev/sda
	/dev/sda: block special (8/0)
	strace file /dev/sda
	openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libmagic.so.1", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/lib/x86_64-linux-gnu/liblzma.so.5", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libbz2.so.1.0", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libbz2.so.1.0", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libz.so.1", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libpthread.so.0", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/usr/lib/locale/locale-archive", O_RDONLY|O_CLOEXEC) = 3
	openat(AT_FDCWD, "/etc/magic", O_RDONLY) = 3
	openat(AT_FDCWD, "/usr/share/misc/magic.mgc", O_RDONLY) = 3
	openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/gconv/gconv-modules.cache", O_RDONLY) = 3

### 3. Предположим, приложение пишет лог в текстовый файл. Этот файл оказался удален (deleted в lsof), однако возможности сигналом сказать приложению переоткрыть файлы или просто перезапустить приложение – нет. Так как приложение продолжает писать в удаленный файл, место на диске постепенно заканчивается. Основываясь на знаниях о перенаправлении потоков предложите способ обнуления открытого удаленного файла (чтобы освободить место на файловой системе).

Перенаправить /dev/null в дескриптор открытого удаленного файла
cat /dev/null > /proc/1625/fd/3
	
	Предположим, заметили, что некоторая файловая система нестандартно растет, находим, что в ней открыто, (предположим, что искомый файл /home/vagrant/.test.txt - открыт и не сохранен в текущем тесте)
	root@vagrant:/home/vagrant# lsof +d /home/vagrant
	COMMAND  PID    USER   FD   TYPE DEVICE SIZE/OFF   NODE NAME
	bash    1119 vagrant  cwd    DIR  253,0     4096 131074 /home/vagrant
	sudo    1126    root  cwd    DIR  253,0     4096 131074 /home/vagrant
	bash    1128    root  cwd    DIR  253,0     4096 131074 /home/vagrant
	bash    1609 vagrant  cwd    DIR  253,0     4096 131074 /home/vagrant
	sudo    1616    root  cwd    DIR  253,0     4096 131074 /home/vagrant
	bash    1618    root  cwd    DIR  253,0     4096 131074 /home/vagrant
	vi      1625    root  cwd    DIR  253,0     4096 131074 /home/vagrant
	vi      1625    root    3u   REG  253,0    12288 131090 /home/vagrant/.test.txt.swp
	lsof    1633    root  cwd    DIR  253,0     4096 131074 /home/vagrant
	lsof    1634    root  cwd    DIR  253,0     4096 131074 /home/vagrant
	root@vagrant:/home/vagrant# cat /dev/null > /home/vagrant/.test.txt.swp
	root@vagrant:/home/vagrant# lsof -p 1625
	COMMAND  PID USER   FD   TYPE DEVICE SIZE/OFF   NODE NAME
	vi      1625 root  cwd    DIR  253,0     4096 131074 /home/vagrant
	vi      1625 root  rtd    DIR  253,0     4096      2 /
	vi      1625 root  txt    REG  253,0  2906824 535065 /usr/bin/vim.basic
	vi      1625 root  mem    REG  253,0    51832 527442 /usr/lib/x86_64-linux-gnu/libnss_files-2.31.so
	vi      1625 root  mem    REG  253,0  5699248 535133 /usr/lib/locale/locale-archive
	vi      1625 root  mem    REG  253,0    47064 534723 /usr/lib/x86_64-linux-gnu/libogg.so.0.8.4
	vi      1625 root  mem    REG  253,0   182344 534725 /usr/lib/x86_64-linux-gnu/libvorbis.so.0.4.8
	vi      1625 root  mem    REG  253,0    14848 527455 /usr/lib/x86_64-linux-gnu/libutil-2.31.so
	vi      1625 root  mem    REG  253,0   108936 526898 /usr/lib/x86_64-linux-gnu/libz.so.1.2.11
	vi      1625 root  mem    REG  253,0   182560 527233 /usr/lib/x86_64-linux-gnu/libexpat.so.1.6.11
	vi      1625 root  mem    REG  253,0    39368 534719 /usr/lib/x86_64-linux-gnu/libltdl.so.7.3.1
	vi      1625 root  mem    REG  253,0   100520 534721 /usr/lib/x86_64-linux-gnu/libtdb.so.1.4.2
	vi      1625 root  mem    REG  253,0    38904 534727 /usr/lib/x86_64-linux-gnu/libvorbisfile.so.3.3.7
	vi      1625 root  mem    REG  253,0   584392 527358 /usr/lib/x86_64-linux-gnu/libpcre2-8.so.0.9.0
	vi      1625 root  mem    REG  253,0  2029224 527432 /usr/lib/x86_64-linux-gnu/libc-2.31.so
	vi      1625 root  mem    REG  253,0   157224 527450 /usr/lib/x86_64-linux-gnu/libpthread-2.31.so
	vi      1625 root  mem    REG  253,0  5449112 524419 /usr/lib/x86_64-linux-gnu/libpython3.8.so.1.0
	vi      1625 root  mem    REG  253,0    18816 527433 /usr/lib/x86_64-linux-gnu/libdl-2.31.so
	vi      1625 root  mem    REG  253,0    22456 534743 /usr/lib/x86_64-linux-gnu/libgpm.so.2
	vi      1625 root  mem    REG  253,0    39088 527180 /usr/lib/x86_64-linux-gnu/libacl.so.1.1.2253
	vi      1625 root  mem    REG  253,0    71680 534729 /usr/lib/x86_64-linux-gnu/libcanberra.so.0.2.5
	vi      1625 root  mem    REG  253,0   163200 527375 /usr/lib/x86_64-linux-gnu/libselinux.so.1
	vi      1625 root  mem    REG  253,0   192032 527400 /usr/lib/x86_64-linux-gnu/libtinfo.so.6.2
	vi      1625 root  mem    REG  253,0  1369352 527435 /usr/lib/x86_64-linux-gnu/libm-2.31.so
	vi      1625 root  mem    REG  253,0   191472 527389 /usr/lib/x86_64-linux-gnu/ld-2.31.so
	vi      1625 root    0u   CHR  136,1      0t0      4 /dev/pts/1
	vi      1625 root    1u   CHR  136,1      0t0      4 /dev/pts/1
	vi      1625 root    2u   CHR  136,1      0t0      4 /dev/pts/1
	vi      1625 root    3u   REG  253,0        0 131090 /home/vagrant/.test.txt.swp
	root@vagrant:/home/vagrant#
	root@vagrant:/home/vagrant# ll /proc/1625/fd/
	total 0
	dr-x------ 2 root root  0 Jan 22 17:27 ./
	dr-xr-xr-x 9 root root  0 Jan 22 17:27 ../
	lrwx------ 1 root root 64 Jan 22 17:27 0 -> /dev/pts/1
	lrwx------ 1 root root 64 Jan 22 17:27 1 -> /dev/pts/1
	lrwx------ 1 root root 64 Jan 22 17:27 2 -> /dev/pts/1
	lrwx------ 1 root root 64 Jan 22 17:27 3 -> /home/vagrant/.test.txt.swp
	root@vagrant:/home/vagrant#
	root@vagrant:/home/vagrant# ll /proc/1625/fd/3
	lrwx------ 1 root root 64 Jan 22 17:27 /proc/1625/fd/3 -> /home/vagrant/.test.txt.swp
	root@vagrant:/home/vagrant#
	
Тест с удаленным файлом
cat /dev/null > /proc/1645/fd/4
	
	root@vagrant:/home/vagrant# lsof +d /home/vagrant
	COMMAND  PID    USER   FD   TYPE DEVICE SIZE/OFF   NODE NAME
	bash    1119 vagrant  cwd    DIR  253,0     4096 131074 /home/vagrant
	sudo    1126    root  cwd    DIR  253,0     4096 131074 /home/vagrant
	bash    1128    root  cwd    DIR  253,0     4096 131074 /home/vagrant
	bash    1609 vagrant  cwd    DIR  253,0     4096 131074 /home/vagrant
	sudo    1616    root  cwd    DIR  253,0     4096 131074 /home/vagrant
	bash    1618    root  cwd    DIR  253,0     4096 131074 /home/vagrant
	vi      1645    root  cwd    DIR  253,0     4096 131074 /home/vagrant
	vi      1645    root    4u   REG  253,0    12288 131090 /home/vagrant/.test.txt.swp
	lsof    1648    root  cwd    DIR  253,0     4096 131074 /home/vagrant
	lsof    1649    root  cwd    DIR  253,0     4096 131074 /home/vagrant
	root@vagrant:/home/vagrant# lsof -p 1645
	COMMAND  PID USER   FD   TYPE DEVICE SIZE/OFF   NODE NAME
	vi      1645 root  cwd    DIR  253,0     4096 131074 /home/vagrant
	vi      1645 root  rtd    DIR  253,0     4096      2 /
	vi      1645 root  txt    REG  253,0  2906824 535065 /usr/bin/vim.basic
	vi      1645 root  mem    REG  253,0    51832 527442 /usr/lib/x86_64-linux-gnu/libnss_files-2.31.so
	vi      1645 root  mem    REG  253,0  5699248 535133 /usr/lib/locale/locale-archive
	vi      1645 root  mem    REG  253,0    47064 534723 /usr/lib/x86_64-linux-gnu/libogg.so.0.8.4
	vi      1645 root  mem    REG  253,0   182344 534725 /usr/lib/x86_64-linux-gnu/libvorbis.so.0.4.8
	vi      1645 root  mem    REG  253,0    14848 527455 /usr/lib/x86_64-linux-gnu/libutil-2.31.so
	vi      1645 root  mem    REG  253,0   108936 526898 /usr/lib/x86_64-linux-gnu/libz.so.1.2.11
	vi      1645 root  mem    REG  253,0   182560 527233 /usr/lib/x86_64-linux-gnu/libexpat.so.1.6.11
	vi      1645 root  mem    REG  253,0    39368 534719 /usr/lib/x86_64-linux-gnu/libltdl.so.7.3.1
	vi      1645 root  mem    REG  253,0   100520 534721 /usr/lib/x86_64-linux-gnu/libtdb.so.1.4.2
	vi      1645 root  mem    REG  253,0    38904 534727 /usr/lib/x86_64-linux-gnu/libvorbisfile.so.3.3.7
	vi      1645 root  mem    REG  253,0   584392 527358 /usr/lib/x86_64-linux-gnu/libpcre2-8.so.0.9.0
	vi      1645 root  mem    REG  253,0  2029224 527432 /usr/lib/x86_64-linux-gnu/libc-2.31.so
	vi      1645 root  mem    REG  253,0   157224 527450 /usr/lib/x86_64-linux-gnu/libpthread-2.31.so
	vi      1645 root  mem    REG  253,0  5449112 524419 /usr/lib/x86_64-linux-gnu/libpython3.8.so.1.0
	vi      1645 root  mem    REG  253,0    18816 527433 /usr/lib/x86_64-linux-gnu/libdl-2.31.so
	vi      1645 root  mem    REG  253,0    22456 534743 /usr/lib/x86_64-linux-gnu/libgpm.so.2
	vi      1645 root  mem    REG  253,0    39088 527180 /usr/lib/x86_64-linux-gnu/libacl.so.1.1.2253
	vi      1645 root  mem    REG  253,0    71680 534729 /usr/lib/x86_64-linux-gnu/libcanberra.so.0.2.5
	vi      1645 root  mem    REG  253,0   163200 527375 /usr/lib/x86_64-linux-gnu/libselinux.so.1
	vi      1645 root  mem    REG  253,0   192032 527400 /usr/lib/x86_64-linux-gnu/libtinfo.so.6.2
	vi      1645 root  mem    REG  253,0  1369352 527435 /usr/lib/x86_64-linux-gnu/libm-2.31.so
	vi      1645 root  mem    REG  253,0   191472 527389 /usr/lib/x86_64-linux-gnu/ld-2.31.so
	vi      1645 root    0u   CHR  136,1      0t0      4 /dev/pts/1
	vi      1645 root    1u   CHR  136,1      0t0      4 /dev/pts/1
	vi      1645 root    2u   CHR  136,1      0t0      4 /dev/pts/1
	vi      1645 root    4u   REG  253,0    12288 131090 /home/vagrant/.test.txt.swp
	root@vagrant:/home/vagrant#
	root@vagrant:/home/vagrant# ll /proc/1645/fd/4
	lrwx------ 1 root root 64 Jan 22 17:41 /proc/1645/fd/4 -> /home/vagrant/.test.txt.swp
	root@vagrant:/home/vagrant#

### 4. Занимают ли зомби-процессы какие-то ресурсы в ОС (CPU, RAM, IO)?

Зомби процессы освобождают свои ресурсы при этом сохраняют запись в таблице процессов.

### 5. В iovisor BCC есть утилита opensnoop:
root@vagrant:~# dpkg -L bpfcc-tools | grep sbin/opensnoop
/usr/sbin/opensnoop-bpfcc
На какие файлы вы увидели вызовы группы open за первую секунду работы утилиты? Воспользуйтесь пакетом bpfcc-tools для Ubuntu 20.04. Дополнительные сведения по установке.
	
	root@vagrant:~# /usr/sbin/opensnoop-bpfcc
	PID    COMM               FD ERR PATH
	686    vminfo              6   0 /var/run/utmp
	473    dbus-daemon        -1   2 /usr/local/share/dbus-1/system-services
	473    dbus-daemon        18   0 /usr/share/dbus-1/system-services
	473    dbus-daemon        -1   2 /lib/dbus-1/system-services
	473    dbus-daemon        18   0 /var/lib/snapd/dbus-1/system-services/
	
### 6. Какой системный вызов использует uname -a? Приведите цитату из man по этому системному вызову, где описывается альтернативное местоположение в /proc, где можно узнать версию ядра и релиз ОС.

	strace uname -a
	...
	uname({sysname="Linux", nodename="LAPTOP-GJ376PE7", ...}) = 0
	...
	man 2 uname | nl
	...
	39         Part of the utsname information is also accessible via /proc/sys/kernel/{ostype, hostname, osrelease, version, domainname}.
	...


### 7. Чем отличается последовательность команд через ; и через && в bash? Например:
### root@netology1:~# test -d /tmp/some_dir; echo Hi
### Hi
### root@netology1:~# test -d /tmp/some_dir && echo Hi
### root@netology1:~#
### Есть ли смысл использовать в bash &&, если применить set -e?

	&& -  условный оператор
	;  - разделитель последовательных команд
	test -d /tmp/some_dir && echo Hi - echo отработает только при успешном заверщении команды test
	set -e - прерывает сессию при любом ненулевом значении исполняемых команд, кроме последней.
	в случае &&  вместе с "set -e" - не имеет смысла, так как при ошибке будет прекращено выполнение команд. 


### 8. Из каких опций состоит режим bash set -euxo pipefail и почему его хорошо было бы использовать в сценариях?

	Повышает деталезацию вывода ошибок(логирования) для сценария, и завершит сценарий при наличии ошибок, на любом этапе выполнения сценария, кроме последней завершающей команды
	-e прерывает выполнение исполнения при ошибке любой команды в последовательности, кроме последней
	-x вывод трейс команд 
	-u неустановленные/не заданные параметры и переменные считаются как ошибки, с выводом в stderr текста ошибки и выполнит завершение неинтерактивного вызова
	-o pipefail возвращает код возврата набора/последовательности команд, ненулевой при последней команды или 0 для успешного выполнения команд.

### 9. Используя -o stat для ps, определите, какой наиболее часто встречающийся статус у процессов в системе. В man ps ознакомьтесь (/PROCESS STATE CODES) что значат дополнительные к основной заглавной буквы статуса процессов. Его можно не учитывать при расчете (считать S, Ss или Ssl равнозначными).

Больше всего процессов со статусом
S - спящие процессы(ожидающие завершения)
R - работающие процессы
Дополнительные символы несут дополнительные характеристики: приоритет, страгицы заблокированные в памяти, многопоточность и др.

	vagrant@vagrant:~$ ps -o stat
	STAT
	Ss
	R+
	man ps
	...
	PROCESS STATE CODES
       Here are the different values that the s, stat and state output specifiers (header "STAT" or "S") will display to describe the state of a process:

               D    uninterruptible sleep (usually IO)
               I    Idle kernel thread
               R    running or runnable (on run queue)
               S    interruptible sleep (waiting for an event to complete)
               T    stopped by job control signal
               t    stopped by debugger during the tracing
               W    paging (not valid since the 2.6.xx kernel)
               X    dead (should never be seen)
               Z    defunct ("zombie") process, terminated but not reaped by its parent
			   For BSD formats and when the stat keyword is used, additional characters may be displayed:

               <    high-priority (not nice to other users)
               N    low-priority (nice to other users)
               L    has pages locked into memory (for real-time and custom IO)
               s    is a session leader
               l    is multi-threaded (using CLONE_THREAD, like NPTL pthreads do)
               +    is in the foreground process group
	

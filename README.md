#Домашнее задание к занятию "3.2. Работа в терминале, лекция 2"

### 1. Какого типа команда cd? Попробуйте объяснить, почему она именно такого типа; опишите ход своих мыслей, если считаете что она могла бы быть другого типа.

Команда cd относится к встроенным командам. Если бы cd была внешней командой, то при смене каталога пришлось бы вызывать новый bash.

	bil@LAPTOP-GJ376PE7:~$ type -a cd uname : ls uname
	cd is a shell builtin
	uname is /usr/bin/uname
	uname is /bin/uname
	: is a shell builtin
	ls is aliased to `ls --color=auto'
	ls is /usr/bin/ls
	ls is /bin/ls
	uname is /usr/bin/uname
	uname is /bin/uname

### 2. Какая альтернатива без pipe команде grep <some_string> <some_file> | wc -l? man grep поможет в ответе на этот вопрос. Ознакомьтесь с документом о других подобных некорректных вариантах использования pipe.

	bil@LAPTOP-GJ376PE7:~$ cat .profile
	# ~/.profile: executed by the command interpreter for login shells.
	# This file is not read by bash(1), if ~/.bash_profile or ~/.bash_login
	# exists.
	# see /usr/share/doc/bash/examples/startup-files for examples.
	# the files are located in the bash-doc package.
	
	# the default umask is set in /etc/profile; for setting the umask
	# for ssh logins, install and configure the libpam-umask package.
	#umask 022
	
	# if running bash
	if [ -n "$BASH_VERSION" ]; then
		# include .bashrc if it exists
		if [ -f "$HOME/.bashrc" ]; then
			. "$HOME/.bashrc"
		fi
	fi
	
	# set PATH so it includes user's private bin if it exists
	if [ -d "$HOME/bin" ] ; then
		PATH="$HOME/bin:$PATH"
	fi
	
	# set PATH so it includes user's private bin if it exists
	if [ -d "$HOME/.local/bin" ] ; then
		PATH="$HOME/.local/bin:$PATH"
	fi
	bil@LAPTOP-GJ376PE7:~$ grep fi .profile -c
	10
	bil@LAPTOP-GJ376PE7:~$ grep fi .profile | wc -l
	10
	bil@LAPTOP-GJ376PE7:~$

### 3. Какой процесс с PID 1 является родителем для всех процессов в вашей виртуальной машине Ubuntu 20.04?

	bil@LAPTOP-GJ376PE7:~$ pstree -p
	init(1)─┬─init(8)───init(9)───bash(10)───pstree(83)
			└─{init}(7)
	bil@LAPTOP-GJ376PE7:~$

### 4. Как будет выглядеть команда, которая перенаправит вывод stderr ls на другую сессию терминала?

открыть сессию
	
	vagrant@vagrant:~$ ls -l \bil 2>/dev/pts/1
	-bash: /dev/pts/1: Permission denied
	vagrant@vagrant:~$ who
	vagrant  pts/0        2022-01-20 18:45 (10.0.2.2)
	vagrant@vagrant:~$
	
открыть другую сессию
	
	vagrant@vagrant:~$ who
	vagrant  pts/0        2022-01-20 18:45 (10.0.2.2)
	vagrant  pts/1        2022-01-20 18:46 (10.0.2.2)
	vagrant@vagrant:~$

### 5. Получится ли одновременно передать команде файл на stdin и вывести ее stdout в другой файл? Приведите работающий пример.

Да, получится

	vagrant@vagrant:~$ cat < .vbox_version > bil_vbox_version
	vagrant@vagrant:~$ cat bil_vbox_version
	6.1.24vagrant@vagrant:~$ cat .vbox_version
	6.1.24vagrant@vagrant:~$

### 6. Получится ли находясь в графическом режиме, вывести данные из PTY в какой-либо из эмуляторов TTY? Сможете ли вы наблюдать выводимые данные?
	
Выводимые данные будут видны

	vagrant@vagrant:~$ echo $SSH_TTY
	/dev/pts/0
	vagrant@vagrant:~$ cat .profile > /dev/tt3
	-bash: /dev/tt3: Permission denied
	vagrant@vagrant:~$ ll /dev/tt3
	ls: cannot access '/dev/tt3': No such file or directory
	vagrant@vagrant:~$ ls /dev/tt3
	ls: cannot access '/dev/tt3': No such file or directory
	vagrant@vagrant:~$ cat .profile > /dev/pts/0
	# ~/.profile: executed by the command interpreter for login shells.
	# This file is not read by bash(1), if ~/.bash_profile or ~/.bash_login
	# exists.
	# see /usr/share/doc/bash/examples/startup-files for examples.
	# the files are located in the bash-doc package.
	
	# the default umask is set in /etc/profile; for setting the umask
	# for ssh logins, install and configure the libpam-umask package.
	#umask 022
	
	# if running bash
	if [ -n "$BASH_VERSION" ]; then
		# include .bashrc if it exists
		if [ -f "$HOME/.bashrc" ]; then
			. "$HOME/.bashrc"
		fi
	fi
	
	# set PATH so it includes user's private bin if it exists
	if [ -d "$HOME/bin" ] ; then
		PATH="$HOME/bin:$PATH"
	fi
	
	# set PATH so it includes user's private bin if it exists
	if [ -d "$HOME/.local/bin" ] ; then
		PATH="$HOME/.local/bin:$PATH"
	fi
	vagrant@vagrant:~$

### 7. Выполните команду bash 5>&1. К чему она приведет? Что будет, если вы выполните echo netology > /proc/$$/fd/5? Почему так происходит?

будет создан дескриптор 5 и перенаправлен на stdout

	vagrant@vagrant:~$ bash 5>&1
	
"echo netology" будет перенаправлен в дескриптор 5, который удет в stdout

	vagrant@vagrant:~$ echo netology > /proc/$$/fd/5
	netology
	vagrant@vagrant:~$

### 8. Получится ли в качестве входного потока для pipe использовать только stderr команды, не потеряв при этом отображение stdout на pty? Напоминаем: по умолчанию через pipe передается только stdout команды слева от | на stdin команды справа. Это можно сделать, поменяв стандартные потоки местами через промежуточный новый дескриптор, который вы научились создавать в предыдущем вопросе.

создан новый дескриптор 4 и перенаправлен в stderr

	bash 4>&2
	vagrant@vagrant:~$ ls bil.txt 4>&2 2>&1 1>&4 | grep no -c
	1

### 9. Что выведет команда cat /proc/$$/environ? Как еще можно получить аналогичный по содержанию вывод?

Будут показаны переменные окружения
аналог - env
	
	vagrant@vagrant:~$ cat /proc/$$/environ	SHELL=/bin/bashLANGUAGE=en_US:PWD=/home/vagrantLOGNAME=vagrantXDG_SESSION_TYPE=ttyMOTD_SHOWN=pamHOME=/home/vagrantLANG=en_US.UTF-8LS_COLORS=rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=00:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arc=01;31:*.arj=01;31:*.taz=01;31:*.lha=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.tzo=01;31:*.t7z=01;31:*.zip=01;31:*.z=01;31:*.dz=01;31:*.gz=01;31:*.lrz=01;31:*.lz=01;31:*.lzo=01;31:*.xz=01;31:*.zst=01;31:*.tzst=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.alz=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.cab=01;31:*.wim=01;31:*.swm=01;31:*.dwm=01;31:*.esd=01;31:*.jpg=01;35:*.jpeg=01;35:*.mjpg=01;35:*.mjpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=00;36:*.au=00;36:*.flac=00;36:*.m4a=00;36:*.mid=00;36:*.midi=00;36:*.mka=00;36:*.mp3=00;36:*.mpc=00;36:*.ogg=00;36:*.ra=00;36:*.wav=00;36:*.oga=00;36:*.opus=00;36:*.spx=00;36:*.xspf=00;36:SSH_CONNECTION=10.0.2.2 61629 10.0.2.15 22LESSCLOSE=/usr/bin/lesspipe %s %sXDG_SESSION_CLASS=userTERM=xterm-256colorLESSOPEN=| /usr/bin/lesspipe %sUSER=vagrantSHLVL=2XDG_SESSION_ID=2XDG_RUNTIME_DIR=/run/user/1000SSH_CLIENT=10.0.2.2 61629 22XDG_DATA_DIRS=/usr/local/share:/usr/share:/var/lib/snapd/desktopPATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/binDBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/1000/busSSH_TTY=/dev/pts/0_=/usr/bin/bashvagrant@vagrant:~$
	
	vagrant@vagrant:~$ env
	SHELL=/bin/bash
	LANGUAGE=en_US:
	PWD=/home/vagrant
	LOGNAME=vagrant
	XDG_SESSION_TYPE=tty
		MOTD_SHOWN=pam
	HOME=/home/vagrant
	LANG=en_US.UTF-8	LS_COLORS=rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=00:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arc=01;31:*.arj=01;31:*.taz=01;31:*.lha=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.tzo=01;31:*.t7z=01;31:*.zip=01;31:*.z=01;31:*.dz=01;31:*.gz=01;31:*.lrz=01;31:*.lz=01;31:*.lzo=01;31:*.xz=01;31:*.zst=01;31:*.tzst=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.alz=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.cab=01;31:*.wim=01;31:*.swm=01;31:*.dwm=01;31:*.esd=01;31:*.jpg=01;35:*.jpeg=01;35:*.mjpg=01;35:*.mjpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=00;36:*.au=00;36:*.flac=00;36:*.m4a=00;36:*.mid=00;36:*.midi=00;36:*.mka=00;36:*.mp3=00;36:*.mpc=00;36:*.ogg=00;36:*.ra=00;36:*.wav=00;36:*.oga=00;36:*.opus=00;36:*.spx=00;36:*.xspf=00;36:
	SSH_CONNECTION=10.0.2.2 61629 10.0.2.15 22
	LESSCLOSE=/usr/bin/lesspipe %s %s
	XDG_SESSION_CLASS=user
	TERM=xterm-256color
	LESSOPEN=| /usr/bin/lesspipe %s
	USER=vagrant
	SHLVL=3
	XDG_SESSION_ID=2
	XDG_RUNTIME_DIR=/run/user/1000
	SSH_CLIENT=10.0.2.2 61629 22
	XDG_DATA_DIRS=/usr/local/share:/usr/share:/var/lib/snapd/desktop
	PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
	DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/1000/bus
	SSH_TTY=/dev/pts/0
	_=/usr/bin/env
	vagrant@vagrant:~$

### 10. Используя man, опишите что доступно по адресам /proc/<PID>/cmdline, /proc/<PID>/exe.

для адреса /proc/<PID>/exe
символьная ссылка для адреса процесса

	vagrant@vagrant:~$ man proc | nl | grep exe
   192         /proc/[pid]/exe
   193                Under Linux 2.2 and later, this file is a symbolic link containing the actual pathname of the executed command.  This
   194                symbolic link can be dereferenced normally; attempting to open it will  open  the  executable.   You  can  even  type
   195                /proc/[pid]/exe  to  run another copy of the same executable that is being run by process [pid].  If the pathname has
   201                Under Linux 2.0 and earlier, /proc/[pid]/exe is a pointer to the binary which was executed, and appears as a symbolic

для адреса /proc/<PID>/cmdline
путь для исполняемого файла процесса

	vagrant@vagrant:~$ man proc | nl | grep cmdline
	18                    directories themselves remain visible).  Sensitive files such as /proc/[pid]/cmdline and  /proc/[pid]/status  are
	156         /proc/[pid]/cmdline
	1117         /proc/cmdline

### 11. Узнайте, какую наиболее старшую версию набора инструкций SSE поддерживает ваш процессор с помощью /proc/cpuinfo.

версия sse4_2

	vagrant@vagrant:~$ grep sse /proc/cpuinfo
	flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx fxsr_opt rdtscp lm constant_tsc rep_good nopl nonstop_tsc cpuid extd_apicid tsc_known_freq pni ssse3 cx16 sse4_1 sse4_2 hypervisor lahf_lm cmp_legacy cr8_legacy 3dnowprefetch ssbd vmmcall fsgsbase arat
	flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx fxsr_opt rdtscp lm constant_tsc rep_good nopl nonstop_tsc cpuid extd_apicid tsc_known_freq pni ssse3 cx16 sse4_1 sse4_2 hypervisor lahf_lm cmp_legacy cr8_legacy 3dnowprefetch ssbd vmmcall fsgsbase arat
	vagrant@vagrant:~$ cat /proc/cpuinfo | grep sse
	flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx fxsr_opt rdtscp lm constant_tsc rep_good nopl nonstop_tsc cpuid extd_apicid tsc_known_freq pni ssse3 cx16 sse4_1 sse4_2 hypervisor lahf_lm cmp_legacy cr8_legacy 3dnowprefetch ssbd vmmcall fsgsbase arat
	flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx fxsr_opt rdtscp lm constant_tsc rep_good nopl nonstop_tsc cpuid extd_apicid tsc_known_freq pni ssse3 cx16 sse4_1 sse4_2 hypervisor lahf_lm cmp_legacy cr8_legacy 3dnowprefetch ssbd vmmcall fsgsbase arat
	vagrant@vagrant:~$

### 12. При открытии нового окна терминала и vagrant ssh создается новая сессия и выделяется pty. Это можно подтвердить командой tty, которая упоминалась в лекции 3.2. Однако:
vagrant@netology1:~$ ssh localhost 'tty'
not a tty
Почитайте, почему так происходит, и как изменить поведение.

Помоему это происходит потому, что ожидается пользователь, а не другой процесс. Для выполнения команды c принудительным созданием псевдотерминала добавить добавить -t

	vagrant@vagrant:~$ ssh -t localhost 'tty'
	The authenticity of host 'localhost (::1)' can't be established.
	ECDSA key fingerprint is SHA256:wSHl+h4vAtTT7mbkj2lbGyxWXWTUf6VUliwpncjwLPM.
	Are you sure you want to continue connecting (yes/no/[fingerprint])? нуы
	Please type 'yes', 'no' or the fingerprint: yes
	Warning: Permanently added 'localhost' (ECDSA) to the list of known hosts.
	vagrant@localhost's password:
	Permission denied, please try again.
	vagrant@localhost's password:
	/dev/pts/1
	Connection to localhost closed.
	vagrant@vagrant:~$

### 13. Бывает, что есть необходимость переместить запущенный процесс из одной сессии в другую. Попробуйте сделать это, воспользовавшись reptyr. Например, так можно перенести в screen процесс, который вы запустили по ошибке в обычной SSH-сессии.

	sudo apt-get install reptyr
	vi /etc/sysctl.d/10-ptrace.conf #установить 
	dkernel.yama.ptrace_scope = 0

	root@vagrant:/home/vagrant# reptyr 13207
	[-] Process 13200 (bash) shares 13207's process group. Unable to attach.
	(This most commonly means that 13207 has suprocesses).
	Unable to attach to pid 13207: Invalid argument
	root@vagrant:/home/vagrant# ps
		PID TTY          TIME CMD
	13243 pts/1    00:00:00 bash
	13253 pts/1    00:00:00 ps
	root@vagrant:/home/vagrant# reptyr 13243
	[-] Timed out waiting for child stop.
	Hangup
	root@vagrant:/home/vagrant# ps
		PID TTY          TIME CMD
		1 ?        00:00:04 systemd
		2 ?        00:00:00 kthreadd
		3 ?        00:00:00 rcu_gp
		4 ?        00:00:00 rcu_par_gp
		6 ?        00:00:00 kworker/0:0H-kblockd


### 14. sudo echo string > /root/new_file не даст выполнить перенаправление под обычным пользователем, так как перенаправлением занимается процесс shell'а, который запущен без sudo под вашим пользователем. Для решения данной проблемы можно использовать конструкцию echo string | sudo tee /root/new_file. Узнайте что делает команда tee и почему в отличие от sudo echo команда с sudo tee будет работать.

tee выводит файл, указаный в качестве параметра, и в stdout.
в данном примере команда получает вывод из stdin, перенаправленный от stdout команды echo, т.к команда запущена от sudo, то у нее есть права за напись в файла

	vagrant@vagrant:~$ sudo echo string > /root/new_file
	bash: /root/new_file: Permission denied
	vagrant@vagrant:~$ echo string | sudo tee /root/new_file
	string
	vagrant@vagrant:~$


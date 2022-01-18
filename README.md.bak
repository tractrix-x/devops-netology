### 1. Установите средство виртуализации Oracle VirtualBox.
	
	sudo apt install virtualbox
	
### 2. Установите средство автоматизации Hashicorp Vagrant.
	
	sudo apt-get install vagrant
	
### 3. В вашем основном окружении подготовьте удобный для дальнейшей работы терминал. Можно предложить:

	iTerm2 в Mac OS X
	Windows Terminal в Windows
	выбрать цветовую схему, размер окна, шрифтов и т.д.
	почитать о кастомизации PS1/применить при желании.
	Несколько популярных проблем:
	
	Добавьте Vagrant в правила исключения перехватывающих трафик для анализа антивирусов, таких как Kaspersky, если у вас возникают связанные с SSL/TLS ошибки,
	MobaXterm может конфликтовать с Vagrant в Windows,
	Vagrant плохо работает с директориями с кириллицей (может быть вашей домашней директорией), тогда можно либо изменить VAGRANT_HOME, либо создать в системе профиль пользователя с английским именем,
	VirtualBox конфликтует с Windows Hyper-V и его необходимо отключить,
	WSL2 использует Hyper-V, поэтому с ним VirtualBox также несовместим,
	аппаратная виртуализация (Intel VT-x, AMD-V) должна быть активна в BIOS,
	в Linux при установке VirtualBox может дополнительно потребоваться пакет linux-headers-generic (debian-based) / kernel-devel (rhel-based).

### 4. С помощью базового файла конфигурации запустите Ubuntu 20.04 в VirtualBox посредством Vagrant:

Создайте директорию, в которой будут храниться конфигурационные файлы Vagrant. В ней выполните vagrant init. Замените содержимое Vagrantfile по умолчанию следующим:

 Vagrant.configure("2") do |config|
 	config.vm.box ### "bento/ubuntu-20.04"
 end
Выполнение в этой директории vagrant up установит провайдер VirtualBox для Vagrant, скачает необходимый образ и запустит виртуальную машину.

vagrant suspend выключит виртуальную машину с сохранением ее состояния (т.е., при следующем vagrant up будут запущены все процессы внутри, которые работали на момент вызова suspend), vagrant halt выключит виртуальную машину штатным образом.
	
	/mnt/d/devops-netology/vagrant project$ ll
	total 12
	drwxrwxrwx 1 bil bil 4096 Dec 19 20:19  ./
	drwxrwxrwx 1 bil bil 4096 Jan 18 18:44  ../
	drwxrwxrwx 1 bil bil 4096 Dec 19 20:19  .vagrant/
	-rwxrwxrwx 1 bil bil   80 Dec 19 20:19  Vagrantfile*
	-rwxrwxrwx 1 bil bil 3080 Dec 19 20:13 'Vagrantfile — orig'*
	-rwxrwxrwx 1 bil bil 3080 Dec 19 20:13  Vagrantfile.bak*
	bil@LAPTOP-GJ376PE7:/mnt/d/devops-netology/vagrant project$ cat Vagrantfile
	Vagrant.configure("2") do |config|
			config.vm.box = "bento/ubuntu-20.04"
	endbil@LAPTOP-GJ376PE7:/mnt/d/devops-netology/vagrant project$
	
### 5. Ознакомьтесь с графическим интерфейсом VirtualBox, посмотрите как выглядит виртуальная машина, которую создал для вас Vagrant, какие аппаратные ресурсы ей выделены. Какие ресурсы выделены по-умолчанию?
	
	оперативная память 1024
	Процессоры 2
	
### 6. Ознакомьтесь с возможностями конфигурации VirtualBox через Vagrantfile: документация. Как добавить оперативной памяти или ресурсов процессора виртуальной машине?
	
	config.vm.provider "virtualbox" do |v|
	v.memory ### 1024
	v.cpus ### 2
	end
	
### 7. Команда vagrant ssh из директории, в которой содержится Vagrantfile, позволит вам оказаться внутри виртуальной машины без каких-либо дополнительных настроек. Попрактикуйтесь в выполнении обсуждаемых команд в терминале Ubuntu.
	
	D:\devops-netology\vagrant project> vagrant ssh
	
### 8.Ознакомиться с разделами man bash, почитать о настройках самого bash:

какой переменной можно задать длину журнала history, и на какой строчке manual это описывается?
что делает директива ignoreboth в bash?


	HISTFILESIZE - максимальное число строк в файле истории для сохранения 
	~$ man bash | nl | grep HISTFILESIZE
	516         HISTFILESIZE
	1848         than the number of lines specified by the value of HISTFILESIZE.  If HISTFILESIZE is unset, or set to null, a non-numeric value, or a numeric value less than zero, the history file is not truncated.
	1854         truncated to contain no more than HISTFILESIZE lines.  If HISTFILESIZE is unset, or set to null, a non-numeric value, or a numeric value less than zero, the history file is not truncated.
	vagrant@vagrant:~$
	
	HISTSIZE - число команд для сохранения
	~$ man bash | nl | grep HISTSIZE
	748                less than zero inhibit truncation.  The shell sets the default value to the  value  of  HISTSIZE  after
	759         HISTSIZE
	2033                HISTSIZE shell variable.  If an attempt is made to set history-size to a non-numeric value, the maximum
	2596         the  list  of commands previously typed.  The value of the HISTSIZE variable is used as the number of commands
	2597         to save in a history list.  The text of the last HISTSIZE commands (default 500) is saved.  The  shell  stores
	2606         FORMAT variable.  When a shell with history enabled exits, the last $HISTSIZE lines are copied from  the  his‐
	vagrant@vagrant:~$
	
	что делает директива ignoreboth в bash?
	ignoreboth это сокращение для 2х директив ignorespace (не сохранять команды начинающиеся с пробела)	and ignoredups (не сохранять команду, если такая уже есть в истории) 	

### 9. В каких сценариях использования применимы скобки {} и на какой строчке man bash это описано?
	
	{} - зарезервированные слова, список, список команд команд, используется в различных условных циклах, условных операторах. В командах выполняет подстановку элементов из списка.
	~$ man bash | nl | grep {
	147         ! case  coproc  do done elif else esac fi for function if in select then until while { } time [[ ]]
	203         { list; }
	
### 11. С учётом ответа на предыдущий вопрос, как создать однократным вызовом touch 100000 файлов? Получится ли аналогичным образом создать 300000? Если нет, то почему?

	touch {000001..100000}.txt - создаст в текущей директории соответсвющее число фалов
	touch {000001..300000}.txt - ошибка: слишком дилинный список аргументов "-bash: /usr/bin/touch: Argument list too long"

### 12. В man bash поищите по /\[\[. Что делает конструкция [[ -d /tmp ]]

	проверяет условие в [[ ]] и возвращает ее статус проверки, в наличие примере [[ -d /tmp ]] проверяется наличие каталога /tmp

### 13. Основываясь на знаниях о просмотре текущих (например, PATH) и установке новых переменных; командах, которые мы рассматривали, добейтесь в выводе type -a bash в виртуальной машине наличия первым пунктом в списке:

bash is /tmp/new_path_directory/bash
bash is /usr/local/bin/bash
bash is /bin/bash
(прочие строки могут отличаться содержимым и порядком) В качестве ответа приведите команды, которые позволили вам добиться указанного вывода или соответствующие скриншоты.

	$ type -a bash
	bash is /usr/bin/bash
	bash is /bin/bash
	
	$ mkdir /tmp/new_path_dir
	$ cp /bin/bash /tmp/new_path_dir/
	$ type -a bash
	bash is /tmp/new_path_dir/bash
	bash is /usr/bin/bash
	bash is /bin/bash
	
### 13. Чем отличается планирование команд с помощью batch и at?
	
	batch - запускает задания если нагрузка в системе будет ниже указанного значения
	at - запускает задание в указанное время
	
### 14. Завершите работу виртуальной машины чтобы не расходовать ресурсы компьютера и/или батарею ноутбука.
	
	vagrant halt
	
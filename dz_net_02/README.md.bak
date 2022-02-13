#	Домашнее задание к занятию "3.7. Компьютерные сети, лекция 2"

### 1. Проверьте список доступных сетевых интерфейсов на вашем компьютере. Какие команды есть для этого в Linux и в Windows?

В Windows команда `ipconfig /all`

	PS C:\Users\Irina> ipconfig /all
	
	Настройка протокола IP для Windows
	
	Имя компьютера  . . . . . . . . . : LAPTOP-GJ376PE7
	Основной DNS-суффикс  . . . . . . :
	Тип узла. . . . . . . . . . . . . : Гибридный
	IP-маршрутизация включена . . . . : Нет
	WINS-прокси включен . . . . . . . : Нет
	Порядок просмотра суффиксов DNS . : Home
	
	Адаптер Ethernet vEthernet (WSL):
	
	DNS-суффикс подключения . . . . . :
	Описание. . . . . . . . . . . . . : Hyper-V Virtual Ethernet Adapter
	Физический адрес. . . . . . . . . : 00-15-5D-D1-EB-EB
	DHCP включен. . . . . . . . . . . : Нет
	Автонастройка включена. . . . . . : Да
	Локальный IPv6-адрес канала . . . : fe80::c0a9:ee21:6aa2:4c67%41(Основной)
	IPv4-адрес. . . . . . . . . . . . : 172.22.16.1(Основной)
	Маска подсети . . . . . . . . . . : 255.255.240.0
	Основной шлюз. . . . . . . . . :
	IAID DHCPv6 . . . . . . . . . . . : 687871325
	DUID клиента DHCPv6 . . . . . . . : 00-01-00-01-28-58-D7-FA-E8-6F-38-71-C2-3B
	DNS-серверы. . . . . . . . . . . : fec0:0:0:ffff::1%1
										fec0:0:0:ffff::2%1
										fec0:0:0:ffff::3%1
	NetBios через TCP/IP. . . . . . . . : Включен
	
	Адаптер Ethernet Ethernet 2:
	
	Состояние среды. . . . . . . . : Среда передачи недоступна.
	DNS-суффикс подключения . . . . . :
	Описание. . . . . . . . . . . . . : Check Point Virtual Network Adapter For SSL Network Extender
	Физический адрес. . . . . . . . . : 54-40-5B-71-A5-0E
	DHCP включен. . . . . . . . . . . : Да
	Автонастройка включена. . . . . . : Да
	
	Адаптер Ethernet VirtualBox Host-Only Network:
	
	DNS-суффикс подключения . . . . . :
	Описание. . . . . . . . . . . . . : VirtualBox Host-Only Ethernet Adapter
	Физический адрес. . . . . . . . . : 0A-00-27-00-00-06
	DHCP включен. . . . . . . . . . . : Нет
	Автонастройка включена. . . . . . : Да
	Локальный IPv6-адрес канала . . . : fe80::55e6:7343:c10d:35b9%6(Основной)
	IPv4-адрес. . . . . . . . . . . . : 192.168.56.1(Основной)
	Маска подсети . . . . . . . . . . : 255.255.255.0
	Основной шлюз. . . . . . . . . :
	IAID DHCPv6 . . . . . . . . . . . : 654966823
	DUID клиента DHCPv6 . . . . . . . : 00-01-00-01-28-58-D7-FA-E8-6F-38-71-C2-3B
	DNS-серверы. . . . . . . . . . . : fec0:0:0:ffff::1%1
										fec0:0:0:ffff::2%1
										fec0:0:0:ffff::3%1
	NetBios через TCP/IP. . . . . . . . : Включен
	
	Адаптер беспроводной локальной сети Беспроводная сеть:
	
	DNS-суффикс подключения . . . . . : Home
	Описание. . . . . . . . . . . . . : Realtek 8822CE Wireless LAN 802.11ac PCI-E NIC
	Физический адрес. . . . . . . . . : E8-6F-38-71-C2-3B
	DHCP включен. . . . . . . . . . . : Да
	Автонастройка включена. . . . . . : Да
	Локальный IPv6-адрес канала . . . : fe80::51b3:f14c:a3ca:f4b4%17(Основной)
	IPv4-адрес. . . . . . . . . . . . : 192.168.1.13(Основной)
	Маска подсети . . . . . . . . . . : 255.255.255.0
	Аренда получена. . . . . . . . . . : 10 февраля 2022 г. 21:43:56
	Срок аренды истекает. . . . . . . . . . : 11 февраля 2022 г. 1:43:53
	Основной шлюз. . . . . . . . . : 192.168.1.1
	DHCP-сервер. . . . . . . . . . . : 192.168.1.1
	IAID DHCPv6 . . . . . . . . . . . : 317222712
	DUID клиента DHCPv6 . . . . . . . : 00-01-00-01-28-58-D7-FA-E8-6F-38-71-C2-3B
	DNS-серверы. . . . . . . . . . . : 192.168.1.1
										192.168.1.1
	NetBios через TCP/IP. . . . . . . . : Включен
	
	Адаптер Ethernet Сетевое подключение Bluetooth:
	
	Состояние среды. . . . . . . . : Среда передачи недоступна.
	DNS-суффикс подключения . . . . . :
	Описание. . . . . . . . . . . . . : Bluetooth Device (Personal Area Network)
	Физический адрес. . . . . . . . . : E8-6F-38-71-C2-3C
	DHCP включен. . . . . . . . . . . : Да
	Автонастройка включена. . . . . . : Да
	PS C:\Users\Irina>

В Linux команда `ip a`

	vagrant@vagrant:~$ ip a
	1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
		link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
		inet 127.0.0.1/8 scope host lo
		valid_lft forever preferred_lft forever
		inet6 ::1/128 scope host
		valid_lft forever preferred_lft forever
	2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
		link/ether 08:00:27:73:60:cf brd ff:ff:ff:ff:ff:ff
		inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic eth0
		valid_lft 86385sec preferred_lft 86385sec
		inet6 fe80::a00:27ff:fe73:60cf/64 scope link
		valid_lft forever preferred_lft forever
	vagrant@vagrant:~$

### 2. Какой протокол используется для распознавания соседа по сетевому интерфейсу? Какой пакет и команды есть в Linux для этого?

	vagrant@vagrant:~$ sudo -i
	root@vagrant:~# apt-get update
	root@vagrant:~# apt-get install lldpd
	
	vagrant@vagrant:~$ lldpctl
	-------------------------------------------------------------------------------
	LLDP neighbors:
	-------------------------------------------------------------------------------
	vagrant@vagrant:
	
`https://deminov.net/enable-lldp-on-the-linux-server/`
	
	root@vagrant:~# systemctl enable lldpd
	Synchronizing state of lldpd.service with SysV service script with /lib/systemd/systemd-sysv-install.
	Executing: /lib/systemd/systemd-sysv-install enable lldpd
	root@vagrant:~# service lldpd start
	root@vagrant:~# echo "# enable agentx for lldp
	> master agentx" >> /etc/snmp/snmpd.conf
	root@vagrant:~# echo 'DAEMON_ARGS="-x -c -s -e"' >> /etc/default/lldpd
	root@vagrant:~# service lldpd restart


### 3. Какая технология используется для разделения L2 коммутатора на несколько виртуальных сетей? Какой пакет и команды есть в Linux для этого? Приведите пример конфига.

Технология VLAN и пакет vlan

Подгрузить модуль 8021q в ядро Linux `https://admin-gu.ru/os/linux/nastrojka-vlan-v-linux-mint-ubuntu-debian`
	vagrant@vagrant:~$ sudo modprobe 8021q

	root@vagrant:~# sudo apt-get install vlan
	Reading package lists... Done
	Building dependency tree
	Reading state information... Done
	The following NEW packages will be installed:
	vlan
	0 upgraded, 1 newly installed, 0 to remove and 125 not upgraded.
	Need to get 11.3 kB of archives.
	After this operation, 50.2 kB of additional disk space will be used.
	Get:1 http://archive.ubuntu.com/ubuntu focal-updates/universe amd64 vlan all 2.0.4ubuntu1.20.04.1 [11.3 kB]
	Fetched 11.3 kB in 1s (12.9 kB/s)
	Selecting previously unselected package vlan.
	(Reading database ... 41736 files and directories currently installed.)
	Preparing to unpack .../vlan_2.0.4ubuntu1.20.04.1_all.deb ...
	Unpacking vlan (2.0.4ubuntu1.20.04.1) ...
	Setting up vlan (2.0.4ubuntu1.20.04.1) ...
	Processing triggers for man-db (2.9.1-1) ...
	root@vagrant:~#

`https://lumpics.ru/configure-network-in-ubuntu/`
	
	vagrant@vagrant:~$ sudo lshw -C network
	*-network
		description: Ethernet interface
		product: 82540EM Gigabit Ethernet Controller
		vendor: Intel Corporation
		physical id: 3
		bus info: pci@0000:00:03.0
		logical name: eth0
		version: 02
		serial: 08:00:27:73:60:cf
		size: 1Gbit/s
		capacity: 1Gbit/s
		width: 32 bits
		clock: 66MHz
		capabilities: pm pcix bus_master cap_list ethernet physical tp 10bt 10bt-fd 100bt 100bt-fd 1000bt-fd autonegotiation
		configuration: autonegotiation=on broadcast=yes driver=e1000 driverversion=7.3.21-k8-NAPI duplex=full ip=10.0.2.15 latency=64 link=yes mingnt=255 multicast=yes port=twisted pair speed=1Gbit/s
		resources: irq:19 memory:f0000000-f001ffff ioport:d010(size=8)
	vagrant@vagrant:~$

Назначим VLAN 9 на интерфейс eth0 и назначил ему новый IP адрес 10.0.0.9/24

	vagrant@vagrant:~$ sudo vconfig add eth0 9
	
	Warning: vconfig is deprecated and might be removed in the future, please migrate to ip(route2) as soon as possible!
	
	vagrant@vagrant:~$
	vagrant@vagrant:~$ sudo ip addr add 10.0.0.9/24 dev eth0.9
	vagrant@vagrant:~$ sudo ip link set up eth0.9
	vagrant@vagrant:~$ ip a
	1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
		link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
		inet 127.0.0.1/8 scope host lo
		valid_lft forever preferred_lft forever
		inet6 ::1/128 scope host
		valid_lft forever preferred_lft forever
	2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
		link/ether 08:00:27:73:60:cf brd ff:ff:ff:ff:ff:ff
		inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic eth0
		valid_lft 84927sec preferred_lft 84927sec
		inet6 fe80::a00:27ff:fe73:60cf/64 scope link
		valid_lft forever preferred_lft forever
	3: eth0.9@eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
		link/ether 08:00:27:73:60:cf brd ff:ff:ff:ff:ff:ff
		inet 10.0.0.9/24 scope global eth0.9
		valid_lft forever preferred_lft forever
		inet6 fe80::a00:27ff:fe73:60cf/64 scope link
		valid_lft forever preferred_lft forever
	vagrant@vagrant:~$


### 4. Какие типы агрегации интерфейсов есть в Linux? Какие опции есть для балансировки нагрузки? Приведите пример конфига.

Типы агрегации интерфейсов в Linux

mode=0 (balance-rr)
Этот режим используется по-умолчанию. balance-rr обеспечивает балансировку нагрузки и отказоустойчивость. В данном режиме пакеты отправляются "по кругу" от первого интерфейса к последнему и сначала. Если выходит из строя один из интерфейсов, пакеты отправляются на остальные оставшиеся.При подключении портов к разным коммутаторам, требует их настройки.

mode=1 (active-backup)
При active-backup один интерфейс работает в активном режиме, остальные в ожидающем. Если активный падает, управление передается одному из ожидающих. Не требует поддержки данной функциональности от коммутатора.

mode=2 (balance-xor)
Передача пакетов распределяется между объединенными интерфейсами по формуле ((MAC-адрес источника) XOR (MAC-адрес получателя)) % число интерфейсов. Один и тот же интерфейс работает с определённым получателем. Режим даёт балансировку нагрузки и отказоустойчивость.

mode=3 (broadcast)
Происходит передача во все объединенные интерфейсы, обеспечивая отказоустойчивость.

mode=4 (802.3ad)
Это динамическое объединение портов. В данном режиме можно получить значительное увеличение пропускной способности как входящего так и исходящего трафика, используя все объединенные интерфейсы. Требует поддержки режима от коммутатора, а так же (иногда) дополнительную настройку коммутатора.

mode=5 (balance-tlb)
Адаптивная балансировка нагрузки. При balance-tlb входящий трафик получается только активным интерфейсом, исходящий - распределяется в зависимости от текущей загрузки каждого интерфейса. Обеспечивается отказоустойчивость и распределение нагрузки исходящего трафика. Не требует специальной поддержки коммутатора.

mode=6 (balance-alb)
Адаптивная балансировка нагрузки (более совершенная). Обеспечивает балансировку нагрузки как исходящего (TLB, transmit load balancing), так и входящего трафика (для IPv4 через ARP). Не требует специальной поддержки коммутатором, но требует возможности изменять MAC-адрес устройства.

Файлы конфигурации сети для Ubuntu
	
	vagrant@vagrant:~$ cat /etc/netplan/01-netcfg.yaml
	network:
	version: 2
	ethernets:
		eth0:
		dhcp4: true
	vagrant@vagrant:~$


### 5. Сколько IP адресов в сети с маской /29 ? Сколько /29 подсетей можно получить из сети с маской /24. Приведите несколько примеров /29 подсетей внутри сети 10.10.10.0/24.

	vagrant@vagrant:~$ ipcalc 10.10.10.0/29
	Address:   10.10.10.0           00001010.00001010.00001010.00000 000
	Netmask:   255.255.255.248 = 29 11111111.11111111.11111111.11111 000
	Wildcard:  0.0.0.7              00000000.00000000.00000000.00000 111
	=>
	Network:   10.10.10.0/29        00001010.00001010.00001010.00000 000
	HostMin:   10.10.10.1           00001010.00001010.00001010.00000 001
	HostMax:   10.10.10.6           00001010.00001010.00001010.00000 110
	Broadcast: 10.10.10.7           00001010.00001010.00001010.00000 111
	Hosts/Net: 6                     Class A, Private Internet
	
	vagrant@vagrant:~$

В этой подсети 8 адресов, включая адрес подсети и бродкаст. На хосты остается 6 адресов.

	vagrant@vagrant:~$ ipcalc 10.10.10.0/24
	Address:   10.10.10.0           00001010.00001010.00001010. 00000000
	Netmask:   255.255.255.0 = 24   11111111.11111111.11111111. 00000000
	Wildcard:  0.0.0.255            00000000.00000000.00000000. 11111111
	=>
	Network:   10.10.10.0/24        00001010.00001010.00001010. 00000000
	HostMin:   10.10.10.1           00001010.00001010.00001010. 00000001
	HostMax:   10.10.10.254         00001010.00001010.00001010. 11111110
	Broadcast: 10.10.10.255         00001010.00001010.00001010. 11111111
	Hosts/Net: 254                   Class A, Private Internet
	
	vagrant@vagrant:~$

В /24 подсети 256 адресов.  Подсеть 10.10.10.0/24 можно разделить на 32 подсети 10.10.10.0/29, например:

	Network: 10.10.10.0/29 
	Network: 10.10.10.8/29
	Network: 10.10.10.16/29
	Network: 10.10.10.24/29
	...
	Network: 10.10.10.248/29

### 6. Задача: вас попросили организовать стык между 2-мя организациями. Диапазоны 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 уже заняты. Из какой подсети допустимо взять частные IP адреса? Маску выберите из расчета максимум 40-50 хостов внутри подсети.

Зарезервированные адреса для использования в частных сетях `http://500hrs.org/tools/tcpip/`
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
Можно использовать диапазон 100.64.0.0 (для использования в сетях сервис-провайдера) `100.64.0.0/10`

Оптимальным решением будут подсети /26 по 64 хоста:

	vagrant@vagrant:~$ ipcalc 100.64.0.0/10 -s 50
	Address:   100.64.0.0           01100100.01 000000.00000000.00000000
	Netmask:   255.192.0.0 = 10     11111111.11 000000.00000000.00000000
	Wildcard:  0.63.255.255         00000000.00 111111.11111111.11111111
	=>
	Network:   100.64.0.0/10        01100100.01 000000.00000000.00000000
	HostMin:   100.64.0.1           01100100.01 000000.00000000.00000001
	HostMax:   100.127.255.254      01100100.01 111111.11111111.11111110
	Broadcast: 100.127.255.255      01100100.01 111111.11111111.11111111
	Hosts/Net: 4194302               Class A
	
	1. Requested size: 50 hosts
	Netmask:   255.255.255.192 = 26 11111111.11111111.11111111.11 000000
	Network:   100.64.0.0/26        01100100.01000000.00000000.00 000000
	HostMin:   100.64.0.1           01100100.01000000.00000000.00 000001
	HostMax:   100.64.0.62          01100100.01000000.00000000.00 111110
	Broadcast: 100.64.0.63          01100100.01000000.00000000.00 111111
	Hosts/Net: 62                    Class A
	
	Needed size:  64 addresses.
	Used network: 100.64.0.0/26
	Unused:
	100.64.0.64/26
	100.64.0.128/25
	100.64.1.0/24
	100.64.2.0/23
	100.64.4.0/22
	100.64.8.0/21
	100.64.16.0/20
	100.64.32.0/19
	100.64.64.0/18
	100.64.128.0/17
	100.65.0.0/16
	100.66.0.0/15
	100.68.0.0/14
	100.72.0.0/13
	100.80.0.0/12
	100.96.0.0/11
	vagrant@vagrant:~$


### 7. Как проверить ARP таблицу в Linux, Windows? Как очистить ARP кеш полностью? Как из ARP таблицы удалить только один нужный IP?

Windows

	PS C:\Users\Irina> arp -a
	
	Интерфейс: 192.168.56.1 --- 0x6
	адрес в Интернете      Физический адрес      Тип
	192.168.56.255        ff-ff-ff-ff-ff-ff     статический
	224.0.0.22            01-00-5e-00-00-16     статический
	224.0.0.113           01-00-5e-00-00-71     статический
	224.0.0.251           01-00-5e-00-00-fb     статический
	224.0.0.252           01-00-5e-00-00-fc     статический
	239.255.255.250       01-00-5e-7f-ff-fa     статический
	
	Интерфейс: 192.168.1.13 --- 0x11
	адрес в Интернете      Физический адрес      Тип
	192.168.1.1           a8-f9-4b-d7-2d-18     динамический
	192.168.1.10          18-83-bf-70-40-c0     динамический
	192.168.1.255         ff-ff-ff-ff-ff-ff     статический
	224.0.0.22            01-00-5e-00-00-16     статический
	224.0.0.113           01-00-5e-00-00-71     статический
	224.0.0.251           01-00-5e-00-00-fb     статический
	239.255.255.250       01-00-5e-7f-ff-fa     статический
	255.255.255.255       ff-ff-ff-ff-ff-ff     статический
	
	Интерфейс: 172.22.16.1 --- 0x29
	адрес в Интернете      Физический адрес      Тип
	172.22.31.255         ff-ff-ff-ff-ff-ff     статический
	224.0.0.22            01-00-5e-00-00-16     статический
	224.0.0.113           01-00-5e-00-00-71     статический
	224.0.0.251           01-00-5e-00-00-fb     статический
	239.255.255.250       01-00-5e-7f-ff-fa     статический
	PS C:\Users\Irina>
	
Оочистить ARP кэш `netsh interface ip delete arpcache`

Linux

	sudo apt install net-tools

Список таблиц

	vagrant@vagrant:~$ arp
	Address                  HWtype  HWaddress           Flags Mask            Iface
	10.0.2.3                 ether   52:54:00:12:35:03   C                     eth0
	_gateway                 ether   52:54:00:12:35:02   C                     eth0
	vagrant@vagrant:~$

Удалить один адрес из таблицы

	vagrant@vagrant:~$ sudo arp -d 10.0.2.3
	vagrant@vagrant:~$ arp
	Address                  HWtype  HWaddress           Flags Mask            Iface
	_gateway                 ether   52:54:00:12:35:02   C                     eth0
	_gateway                 ether   52:54:00:12:35:02   C                     eth0

Удалить все записи

	vagrant@vagrant:~$ sudo ip neigh flush all
	vagrant@vagrant:~$ arp
	Address                  HWtype  HWaddress           Flags Mask            Iface
	_gateway                 ether   52:54:00:12:35:02   C                     eth0
	_gateway                 ether   52:54:00:12:35:02   C                     eth0
	vagrant@vagrant:~$
	
	
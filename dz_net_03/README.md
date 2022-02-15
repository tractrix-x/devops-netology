# Домашнее задание к занятию "3.8. Компьютерные сети, лекция 3"

### 1. Подключитесь к публичному маршрутизатору в интернет. Найдите маршрут к вашему публичному IP
	
	telnet route-views.routeviews.org
	Username: rviews
	show ip route x.x.x.x/32
	show bgp x.x.x.x/32

Выведем содержимое таблицы маршрутизации

	route-views>show ip route 178.47.119.88
	Routing entry for 178.47.96.0/19
	Known via "bgp 6447", distance 20, metric 0
	Tag 2497, type external
	Last update from 202.232.0.2 5d10h ago
	Routing Descriptor Blocks:
	* 202.232.0.2, from 202.232.0.2, 5d10h ago
		Route metric is 0, traffic share count is 1
		AS Hops 2
		Route tag 2497
		MPLS label: none
	route-views>

Записи в таблице маршрутизации протокола пограничного шлюза (BGP) 	

	route-views>show bgp 178.47.119.88
	BGP routing table entry for 178.47.96.0/19, version 302094790
	Paths: (23 available, best #17, table default)
	Not advertised to any peer
	Refresh Epoch 1
	701 1273 12389
		137.39.3.55 from 137.39.3.55 (137.39.3.55)
		Origin IGP, localpref 100, valid, external
		path 7FE0E0790FA8 RPKI State valid
		rx pathid: 0, tx pathid: 0
	Refresh Epoch 1
	3549 3356 12389
		208.51.134.254 from 208.51.134.254 (67.16.168.191)
		Origin IGP, metric 0, localpref 100, valid, external
		Community: 3356:2 3356:22 3356:100 3356:123 3356:501 3356:901 3356:2065 3549:2581 3549:30840
		path 7FE0461AD5A8 RPKI State valid
		rx pathid: 0, tx pathid: 0
	Refresh Epoch 1
	53767 14315 6453 6453 3356 12389
		162.251.163.2 from 162.251.163.2 (162.251.162.3)
		Origin IGP, localpref 100, valid, external
		Community: 14315:5000 53767:5000
		path 7FE0F0EAD968 RPKI State valid
		rx pathid: 0, tx pathid: 0
	Refresh Epoch 1
	3356 12389
		4.68.4.46 from 4.68.4.46 (4.69.184.201)
		Origin IGP, metric 0, localpref 100, valid, external
		Community: 3356:2 3356:22 3356:100 3356:123 3356:501 3356:901 3356:2065
		path 7FE18A7FD748 RPKI State valid
		rx pathid: 0, tx pathid: 0
	Refresh Epoch 1
	8283 1299 12389
		94.142.247.3 from 94.142.247.3 (94.142.247.3)
		Origin IGP, metric 0, localpref 100, valid, external
		Community: 1299:30000 8283:1 8283:101 8283:103
		unknown transitive attribute: flag 0xE0 type 0x20 length 0x24
			value 0000 205B 0000 0000 0000 0001 0000 205B
				0000 0005 0000 0001 0000 205B 0000 0005
				0000 0003
		path 7FE0AE2E1838 RPKI State valid
		rx pathid: 0, tx pathid: 0
	Refresh Epoch 1
	4901 6079 1299 12389
		162.250.137.254 from 162.250.137.254 (162.250.137.254)
		Origin IGP, localpref 100, valid, external
		Community: 65000:10100 65000:10300 65000:10400
		path 7FE04E4D5DC0 RPKI State valid
		rx pathid: 0, tx pathid: 0
	Refresh Epoch 1
	57866 3356 12389
		37.139.139.17 from 37.139.139.17 (37.139.139.17)
		Origin IGP, metric 0, localpref 100, valid, external
		Community: 3356:2 3356:22 3356:100 3356:123 3356:501 3356:901 3356:2065
		path 7FE151898898 RPKI State valid
		rx pathid: 0, tx pathid: 0
	Refresh Epoch 1
	852 3491 12389
		154.11.12.212 from 154.11.12.212 (96.1.209.43)
		Origin IGP, metric 0, localpref 100, valid, external
		path 7FE0848FBB48 RPKI State valid
		rx pathid: 0, tx pathid: 0
	Refresh Epoch 1
	20912 3257 174 12389
		212.66.96.126 from 212.66.96.126 (212.66.96.126)
		Origin IGP, localpref 100, valid, external
		Community: 3257:8070 3257:30155 3257:50001 3257:53900 3257:53902 20912:65004
		path 7FE087CF6870 RPKI State valid
		rx pathid: 0, tx pathid: 0
	Refresh Epoch 1
	3303 12389
		217.192.89.50 from 217.192.89.50 (138.187.128.158)
		Origin IGP, localpref 100, valid, external
		Community: 3303:1004 3303:1006 3303:1030 3303:3056
		path 7FE10A333208 RPKI State valid
		rx pathid: 0, tx pathid: 0
	Refresh Epoch 1
	101 3491 12389
		209.124.176.223 from 209.124.176.223 (209.124.176.223)
		Origin IGP, localpref 100, valid, external
		Community: 101:20300 101:22100 3491:400 3491:415 3491:9001 3491:9080 3491:9081 3491:9087 3491:62210 3491:62220
		path 7FE0D4029C40 RPKI State valid
		rx pathid: 0, tx pathid: 0
	Refresh Epoch 1
	3333 1103 12389
		193.0.0.56 from 193.0.0.56 (193.0.0.56)
		Origin IGP, localpref 100, valid, external
		path 7FE1702ADFE8 RPKI State valid
		rx pathid: 0, tx pathid: 0
	Refresh Epoch 1
	7018 1299 12389
		12.0.1.63 from 12.0.1.63 (12.0.1.63)
		Origin IGP, localpref 100, valid, external
		Community: 7018:5000 7018:37232
		path 7FE15ED6AEF0 RPKI State valid
		rx pathid: 0, tx pathid: 0
	Refresh Epoch 1
	3561 3910 3356 12389
		206.24.210.80 from 206.24.210.80 (206.24.210.80)
		Origin IGP, localpref 100, valid, external
		path 7FE15F038DE8 RPKI State valid
		rx pathid: 0, tx pathid: 0
	Refresh Epoch 1
	1351 6939 12389
		132.198.255.253 from 132.198.255.253 (132.198.255.253)
		Origin IGP, localpref 100, valid, external
		path 7FE128DD40F0 RPKI State valid
		rx pathid: 0, tx pathid: 0
	Refresh Epoch 1
	20130 6939 12389
		140.192.8.16 from 140.192.8.16 (140.192.8.16)
		Origin IGP, localpref 100, valid, external
		path 7FE0FD985DE8 RPKI State valid
		rx pathid: 0, tx pathid: 0
	Refresh Epoch 2
	2497 12389
		202.232.0.2 from 202.232.0.2 (58.138.96.254)
		Origin IGP, localpref 100, valid, external, best
		path 7FE169383330 RPKI State valid
		rx pathid: 0, tx pathid: 0x0
	Refresh Epoch 1
	7660 2516 12389
		203.181.248.168 from 203.181.248.168 (203.181.248.168)
		Origin IGP, localpref 100, valid, external
		Community: 2516:1050 7660:9001
		path 7FE0AD1CA0C0 RPKI State valid
		rx pathid: 0, tx pathid: 0
	Refresh Epoch 1
	49788 12552 12389
		91.218.184.60 from 91.218.184.60 (91.218.184.60)
		Origin IGP, localpref 100, valid, external
		Community: 12552:12000 12552:12100 12552:12101 12552:22000
		Extended Community: 0x43:100:1
		path 7FE10413BBA0 RPKI State valid
		rx pathid: 0, tx pathid: 0
	Refresh Epoch 1
	1221 4637 12389
		203.62.252.83 from 203.62.252.83 (203.62.252.83)
		Origin IGP, localpref 100, valid, external
		path 7FE0DFAE8A98 RPKI State valid
		rx pathid: 0, tx pathid: 0
	Refresh Epoch 1
	3257 1299 12389
		89.149.178.10 from 89.149.178.10 (213.200.83.26)
		Origin IGP, metric 10, localpref 100, valid, external
		Community: 3257:8794 3257:30052 3257:50001 3257:54900 3257:54901
		path 7FE11E45FC10 RPKI State valid
		rx pathid: 0, tx pathid: 0
	Refresh Epoch 1
	6939 12389
		64.71.137.241 from 64.71.137.241 (216.218.252.164)
		Origin IGP, localpref 100, valid, external
		path 7FE188E419C8 RPKI State valid
		rx pathid: 0, tx pathid: 0
	Refresh Epoch 1
	19214 174 12389
		208.74.64.40 from 208.74.64.40 (208.74.64.40)
		Origin IGP, localpref 100, valid, external
		Community: 174:21101 174:22005
		path 7FE0D048B938 RPKI State valid
		rx pathid: 0, tx pathid: 0
		

### 2. Создайте dummy0 интерфейс в Ubuntu. Добавьте несколько статических маршрутов. Проверьте таблицу маршрутизации.

`https://ixnfo.com/sozdanie-dummy-interfeysov-v-linux.html`

Загрузим модуль «dummy», добавим опцию «numdummies=2» чтобы сразу создалось два интерфейса dummyX

	sudo modprobe -v dummy numdummies=2
	
Проверим загрузку модуля

	vagrant@vagrant:~$ lsmod | grep dummy
	dummy                  16384  0
	vagrant@vagrant:~$

Проверим создание интерфейсов

	vagrant@vagrant:~$ ifconfig -a | grep dummy
	dummy0: flags=130<BROADCAST,NOARP>  mtu 1500
	dummy1: flags=130<BROADCAST,NOARP>  mtu 1500
	vagrant@vagrant:~$

Добавим IP адрес с интерфейса dummy0 

sudo ip addr add 178.47.119.88/24 dev dummy0
sudo ip addr add 83.149.37.54/24 dev dummy0

### 3. Проверьте открытые TCP порты в Ubuntu, какие протоколы и приложения используют эти порты? Приведите несколько примеров.

Проверить открытые TCP порты в Ubuntu можно `netstat -pnltu` или `ss -ltupn` c `https://losst.ru/otkrytye-porty-ubuntu`
Опция 
-l сообщает, что нужно посмотреть прослушиваемые порты, 
-p  показывает имя программы, 
-t отображают TCP 
-u отображают UDP порты
-n показывает ip адреса в числовом виде.

	vagrant@vagrant:~$ netstat -pnlt
	(Not all processes could be identified, non-owned process info
	will not be shown, you would have to be root to see it all.)
	Active Internet connections (only servers)
	Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
	tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      -
	tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -
	tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -
	tcp6       0      0 :::111                  :::*                    LISTEN      -
	tcp6       0      0 :::22                   :::*                    LISTEN      -
	vagrant@vagrant:~$

`ss -ltpn` со значениями портов

	vagrant@vagrant:~$ ss -ltpn
	State         Recv-Q        Send-Q               Local Address:Port               Peer Address:Port       Process
	LISTEN        0             4096                       0.0.0.0:111                     0.0.0.0:*
	LISTEN        0             4096                 127.0.0.53%lo:53                      0.0.0.0:*
	LISTEN        0             128                        0.0.0.0:22                      0.0.0.0:*
	LISTEN        0             4096                          [::]:111                        [::]:*
	LISTEN        0             128                           [::]:22                         [::]:*
	vagrant@vagrant:~$

В данном случае это внутренние порты, нет приложений использующих их. Используемые протоколы `tcp`

`Здесь в первом столбце отображается протокол, затем два столбца - это данные, которые нам ничего полезного не говорят, а за ними уже идут локальный и внешний адреса. Если локальный адрес - 127.0.0.1, то это значит, что сервис доступен только на этом компьютере, а значение 0.0.0.0 или :: означает любой адрес, к таким сервисам могут подключаться из сети. В нашем примере это Apache и systemd-resolvd.`

### 4. Проверьте используемые UDP сокеты в Ubuntu, какие протоколы и приложения используют эти порты?

Проверьте используемые UDP сокеты в Ubuntu. По `https://losst.ru/monitoring-setevyh-podklyuchenij-v-linux` команда `sudo ss -ua` 
`Для отображения UDP сокетов используйте опцию u. По умолчанию будут показаны только подключенные соединения. Если хотите получить все, нужно использовать опцию a. Поскольку UDP, это протокол без постоянного соединения, то без опции -a мы ничего не увидим.`

	vagrant@vagrant:~$ sudo ss -ua
	State        Recv-Q       Send-Q               Local Address:Port                 Peer Address:Port       Process
	UNCONN       0            0                    127.0.0.53%lo:domain                    0.0.0.0:*
	UNCONN       0            0                   10.0.2.15%eth0:bootpc                    0.0.0.0:*
	UNCONN       0            0                          0.0.0.0:sunrpc                    0.0.0.0:*
	UNCONN       0            0                             [::]:sunrpc                       [::]:*
	vagrant@vagrant:~$

`ss -ltupn` со значениями портов

	vagrant@vagrant:~$ ss -ltupn
	Netid      State       Recv-Q      Send-Q            Local Address:Port             Peer Address:Port      Process
	udp        UNCONN      0           0                 127.0.0.53%lo:53                    0.0.0.0:*
	udp        UNCONN      0           0                10.0.2.15%eth0:68                    0.0.0.0:*
	udp        UNCONN      0           0                       0.0.0.0:111                   0.0.0.0:*
	udp        UNCONN      0           0                          [::]:111                      [::]:*
	tcp        LISTEN      0           4096                    0.0.0.0:111                   0.0.0.0:*
	tcp        LISTEN      0           4096              127.0.0.53%lo:53                    0.0.0.0:*
	tcp        LISTEN      0           128                     0.0.0.0:22                    0.0.0.0:*
	tcp        LISTEN      0           4096                       [::]:111                      [::]:*
	tcp        LISTEN      0           128                        [::]:22                       [::]:*
	vagrant@vagrant:~$

Нет внешних приложений для этих портов.

### 5. Используя diagrams.net, создайте L3 диаграмму вашей домашней сети или любой другой сети, с которой вы работали.

 ( img/home_netvork.png )


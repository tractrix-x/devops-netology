# Домашнее задание к занятию "3.5. Файловые системы"

### 1. Узнайте о sparse (разряженных) файлах.

	https://ru.wikipedia.org/wiki/%D0%A0%D0%B0%D0%B7%D1%80%D0%B5%D0%B6%D1%91%D0%BD%D0%BD%D1%8B%D0%B9_%D1%84%D0%B0%D0%B9%D0%BB
	Спасибо, это очень интересный материал. Ранее этого не знала. 
	По прочтению, у меня возникла ассоциация по sparse файлам ОС и файлам БД, когда (например в ASE) в заголовке файла (устройства БД) хранятся его размеры, в том числе в страницах, и хранятся данные об пустых страницах этого устройства. 

### 2. Могут ли файлы, являющиеся жесткой ссылкой на один объект, иметь разные права доступа и владельца? Почему?

	Так как жесткая ссылка и объект источник имеют один и тот же inode, то права доступа и владелец будут совпадать.

### 3. Сделайте vagrant destroy на имеющийся инстанс Ubuntu. Замените содержимое Vagrantfile следующим:
	
	Vagrant.configure("2") do |config|
	config.vm.box = "bento/ubuntu-20.04"
	config.vm.provider :virtualbox do |vb|
		lvm_experiments_disk0_path = "/tmp/lvm_experiments_disk0.vmdk"
		lvm_experiments_disk1_path = "/tmp/lvm_experiments_disk1.vmdk"
		vb.customize ['createmedium', '--filename', lvm_experiments_disk0_path, '--size', 2560]
		vb.customize ['createmedium', '--filename', lvm_experiments_disk1_path, '--size', 2560]
		vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', lvm_experiments_disk0_path]
		vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 2, '--device', 0, '--type', 'hdd', '--medium', lvm_experiments_disk1_path]
	end
	end
	
Данная конфигурация создаст новую виртуальную машину с двумя дополнительными неразмеченными дисками по 2.5 Гб.

	vagrant destroy -f 
	
	vagrant@vagrant:~$ df -h
	Filesystem                  Size  Used Avail Use% Mounted on
	udev                        447M     0  447M   0% /dev
	tmpfs                        99M  660K   98M   1% /run
	/dev/mapper/vgvagrant-root   62G  1.5G   57G   3% /
	tmpfs                       491M     0  491M   0% /dev/shm
	tmpfs                       5.0M     0  5.0M   0% /run/lock
	tmpfs                       491M     0  491M   0% /sys/fs/cgroup
	/dev/sda1                   511M  4.0K  511M   1% /boot/efi
	vagrant                     382G  3.8G  378G   1% /vagrant
	tmpfs                        99M     0   99M   0% /run/user/1000
	
	vagrant@vagrant:~$ lsblk
	NAME                 MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
	sda                    8:0    0   64G  0 disk
	├─sda1                 8:1    0  512M  0 part /boot/efi
	├─sda2                 8:2    0    1K  0 part
	└─sda5                 8:5    0 63.5G  0 part
	├─vgvagrant-root   253:0    0 62.6G  0 lvm  /
	└─vgvagrant-swap_1 253:1    0  980M  0 lvm  [SWAP]
	sdb                    8:16   0  2.5G  0 disk
	sdc                    8:32   0  2.5G  0 disk
	vagrant@vagrant:~$
	
	vagrant@vagrant:~$ sudo fdisk -l
	Disk /dev/sda: 64 GiB, 68719476736 bytes, 134217728 sectors
	Disk model: VBOX HARDDISK
	Units: sectors of 1 * 512 = 512 bytes
	Sector size (logical/physical): 512 bytes / 512 bytes
	I/O size (minimum/optimal): 512 bytes / 512 bytes
	Disklabel type: dos
	Disk identifier: 0x3f94c461
	
	Device     Boot   Start       End   Sectors  Size Id Type
	/dev/sda1  *       2048   1050623   1048576  512M  b W95 FAT32
	/dev/sda2       1052670 134215679 133163010 63.5G  5 Extended
	/dev/sda5       1052672 134215679 133163008 63.5G 8e Linux LVM
	
	
	Disk /dev/sdb: 2.51 GiB, 2684354560 bytes, 5242880 sectors
	Disk model: VBOX HARDDISK
	Units: sectors of 1 * 512 = 512 bytes
	Sector size (logical/physical): 512 bytes / 512 bytes
	I/O size (minimum/optimal): 512 bytes / 512 bytes
	
	
	Disk /dev/sdc: 2.51 GiB, 2684354560 bytes, 5242880 sectors
	Disk model: VBOX HARDDISK
	Units: sectors of 1 * 512 = 512 bytes
	Sector size (logical/physical): 512 bytes / 512 bytes
	I/O size (minimum/optimal): 512 bytes / 512 bytes
	
	
	Disk /dev/mapper/vgvagrant-root: 62.55 GiB, 67150807040 bytes, 131153920 sectors
	Units: sectors of 1 * 512 = 512 bytes
	Sector size (logical/physical): 512 bytes / 512 bytes
	I/O size (minimum/optimal): 512 bytes / 512 bytes
	
	
	Disk /dev/mapper/vgvagrant-swap_1: 980 MiB, 1027604480 bytes, 2007040 sectors
	Units: sectors of 1 * 512 = 512 bytes
	Sector size (logical/physical): 512 bytes / 512 bytes
	I/O size (minimum/optimal): 512 bytes / 512 bytes
	vagrant@vagrant:~$
	

### 4. Используя fdisk, разбейте первый диск на 2 раздела: 2 Гб, оставшееся пространство.

	vagrant@vagrant:~$ lsblk
	NAME                 MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
	sda                    8:0    0   64G  0 disk
	├─sda1                 8:1    0  512M  0 part /boot/efi
	├─sda2                 8:2    0    1K  0 part
	└─sda5                 8:5    0 63.5G  0 part
	├─vgvagrant-root   253:0    0 62.6G  0 lvm  /
	└─vgvagrant-swap_1 253:1    0  980M  0 lvm  [SWAP]
	sdb                    8:16   0  2.5G  0 disk
	sdc                    8:32   0  2.5G  0 disk
	
	vagrant@vagrant:~$ sudo fdisk /dev/sdb
	
	Welcome to fdisk (util-linux 2.34).
	Changes will remain in memory only, until you decide to write them.
	Be careful before using the write command.
	
	Device does not contain a recognized partition table.
	Created a new DOS disklabel with disk identifier 0x475357eb.
	
	Command (m for help): n
	Partition type
	p   primary (0 primary, 0 extended, 4 free)
	e   extended (container for logical partitions)
	Select (default p): p
	Partition number (1-4, default 1):
	First sector (2048-5242879, default 2048):
	Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-5242879, default 5242879): +2G
	
	Created a new partition 1 of type 'Linux' and of size 2 GiB.
	
	Command (m for help): n
	Partition type
	p   primary (1 primary, 0 extended, 3 free)
	e   extended (container for logical partitions)
	Select (default p):
	
	Using default response p.
	Partition number (2-4, default 2):
	First sector (4196352-5242879, default 4196352):
	Last sector, +/-sectors or +/-size{K,M,G,T,P} (4196352-5242879, default 5242879):
	
	Created a new partition 2 of type 'Linux' and of size 511 MiB.
	
	Command (m for help): w
	The partition table has been altered.
	Calling ioctl() to re-read partition table.
	Syncing disks.
	
	vagrant@vagrant:~$ lsblk
	NAME                 MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
	sda                    8:0    0   64G  0 disk
	├─sda1                 8:1    0  512M  0 part /boot/efi
	├─sda2                 8:2    0    1K  0 part
	└─sda5                 8:5    0 63.5G  0 part
	├─vgvagrant-root   253:0    0 62.6G  0 lvm  /
	└─vgvagrant-swap_1 253:1    0  980M  0 lvm  [SWAP]
	sdb                    8:16   0  2.5G  0 disk
	├─sdb1                 8:17   0    2G  0 part
	└─sdb2                 8:18   0  511M  0 part
	sdc                    8:32   0  2.5G  0 disk
	vagrant@vagrant:~$
	

### 5. Используя sfdisk, перенесите данную таблицу разделов на второй диск.


	vagrant@vagrant:~$ sudo sfdisk -d /dev/sdb | sudo sfdisk --force /dev/sdc
	Checking that no-one is using this disk right now ... OK
	
	Disk /dev/sdc: 2.51 GiB, 2684354560 bytes, 5242880 sectors
	Disk model: VBOX HARDDISK
	Units: sectors of 1 * 512 = 512 bytes
	Sector size (logical/physical): 512 bytes / 512 bytes
	I/O size (minimum/optimal): 512 bytes / 512 bytes
	
	>>> Script header accepted.
	>>> Script header accepted.
	>>> Script header accepted.
	>>> Script header accepted.
	>>> Created a new DOS disklabel with disk identifier 0x475357eb.
	/dev/sdc1: Created a new partition 1 of type 'Linux' and of size 2 GiB.
	/dev/sdc2: Created a new partition 2 of type 'Linux' and of size 511 MiB.
	/dev/sdc3: Done.
	
	New situation:
	Disklabel type: dos
	Disk identifier: 0x475357eb
	
	Device     Boot   Start     End Sectors  Size Id Type
	/dev/sdc1          2048 4196351 4194304    2G 83 Linux
	/dev/sdc2       4196352 5242879 1046528  511M 83 Linux
	
	The partition table has been altered.
	Calling ioctl() to re-read partition table.
	Syncing disks.
	vagrant@vagrant:~$
	
	vagrant@vagrant:~$ lsblk
	NAME                 MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
	sda                    8:0    0   64G  0 disk
	├─sda1                 8:1    0  512M  0 part /boot/efi
	├─sda2                 8:2    0    1K  0 part
	└─sda5                 8:5    0 63.5G  0 part
	├─vgvagrant-root   253:0    0 62.6G  0 lvm  /
	└─vgvagrant-swap_1 253:1    0  980M  0 lvm  [SWAP]
	sdb                    8:16   0  2.5G  0 disk
	├─sdb1                 8:17   0    2G  0 part
	└─sdb2                 8:18   0  511M  0 part
	sdc                    8:32   0  2.5G  0 disk
	├─sdc1                 8:33   0    2G  0 part
	└─sdc2                 8:34   0  511M  0 part
	vagrant@vagrant:~$
	

### 6. Соберите mdadm RAID1 на паре разделов 2 Гб.

Создание рейда
	vagrant@vagrant:~$ sudo mdadm --create --verbose /dev/md0 -l 1 -n 2 /dev/sd{b1,c1}
	mdadm: Note: this array has metadata at the start and
    may not be suitable as a boot device.  If you plan to
    store '/boot' on this device please ensure that
    your boot-loader understands md/v1.x metadata, or use
    --metadata=0.90
	mdadm: size set to 2094080K
	Continue creating array?
	Continue creating array? (y/n) y
	mdadm: Defaulting to version 1.2 metadata
	mdadm: array /dev/md0 started.
	
	vagrant@vagrant:~$ cat /proc/mdstat
	Personalities : [linear] [multipath] [raid0] [raid1] [raid6] [raid5] [raid4] [raid10]
	md0 : active raid1 sdc1[1] sdb1[0]
		2094080 blocks super 1.2 [2/2] [UU]
	
	unused devices: <none>
	vagrant@vagrant:~$		

### 7. Соберите mdadm RAID0 на второй паре маленьких разделов.

	vagrant@vagrant:~$ sudo mdadm --create --verbose /dev/md1 -l 0 -n 2 /dev/sd{b2,c2}
	mdadm: chunk size defaults to 512K
	mdadm: Defaulting to version 1.2 metadata
	mdadm: array /dev/md1 started.
	
	vagrant@vagrant:~$ lsblk
	NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
	sda                    8:0    0   64G  0 disk
	├─sda1                 8:1    0  512M  0 part  /boot/efi
	├─sda2                 8:2    0    1K  0 part
	└─sda5                 8:5    0 63.5G  0 part
	├─vgvagrant-root   253:0    0 62.6G  0 lvm   /
	└─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]
	sdb                    8:16   0  2.5G  0 disk
	├─sdb1                 8:17   0    2G  0 part
	│ └─md0                9:0    0    2G  0 raid1
	└─sdb2                 8:18   0  511M  0 part
	└─md1                9:1    0 1018M  0 raid0
	sdc                    8:32   0  2.5G  0 disk
	├─sdc1                 8:33   0    2G  0 part
	│ └─md0                9:0    0    2G  0 raid1
	└─sdc2                 8:34   0  511M  0 part
	└─md1                9:1    0 1018M  0 raid0
	vagrant@vagrant:~$
		
	vagrant@vagrant:~$ cat /proc/mdstat
	Personalities : [linear] [multipath] [raid0] [raid1] [raid6] [raid5] [raid4] [raid10]
	md1 : active raid0 sdc2[1] sdb2[0]
		1042432 blocks super 1.2 512k chunks
	
	md0 : active raid1 sdc1[1] sdb1[0]
		2094080 blocks super 1.2 [2/2] [UU]
	
	unused devices: <none>
	vagrant@vagrant:~$

### 8. Создайте 2 независимых PV на получившихся md-устройствах.

	vagrant@vagrant:~$ sudo pvcreate /dev/md1 /dev/md0
	Physical volume "/dev/md1" successfully created.
	Physical volume "/dev/md0" successfully created.
	vagrant@vagrant:~$

### 9. Создайте общую volume-group на этих двух PV.

	vagrant@vagrant:~$ sudo vgcreate vg1 /dev/md1 /dev/md0
	Volume group "vg1" successfully created
	vagrant@vagrant:~$
	
	vagrant@vagrant:~$ sudo vgdisplay
	--- Volume group ---
	VG Name               vgvagrant
	System ID
	Format                lvm2
	Metadata Areas        1
	Metadata Sequence No  3
	VG Access             read/write
	VG Status             resizable
	MAX LV                0
	Cur LV                2
	Open LV               2
	Max PV                0
	Cur PV                1
	Act PV                1
	VG Size               <63.50 GiB
	PE Size               4.00 MiB
	Total PE              16255
	Alloc PE / Size       16255 / <63.50 GiB
	Free  PE / Size       0 / 0
	VG UUID               PaBfZ0-3I0c-iIdl-uXKt-JL4K-f4tT-kzfcyE
	
	--- Volume group ---
	VG Name               vg1
	System ID
	Format                lvm2
	Metadata Areas        2
	Metadata Sequence No  1
	VG Access             read/write
	VG Status             resizable
	MAX LV                0
	Cur LV                0
	Open LV               0
	Max PV                0
	Cur PV                2
	Act PV                2
	VG Size               <2.99 GiB
	PE Size               4.00 MiB
	Total PE              765
	Alloc PE / Size       0 / 0
	Free  PE / Size       765 / <2.99 GiB
	VG UUID               yKm2k4-xSs3-y7zf-tgYx-JfPd-TPCd-gBhVjl
	
	vagrant@vagrant:~$

### 10. Создайте LV размером 100 Мб, указав его расположение на PV с RAID0.

	vagrant@vagrant:~$ sudo lvcreate -L 100M vg1 /dev/md1
	Logical volume "lvol0" created.
	vagrant@vagrant:~$
	
	vagrant@vagrant:~$ lsblk
	NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
	sda                    8:0    0   64G  0 disk
	├─sda1                 8:1    0  512M  0 part  /boot/efi
	├─sda2                 8:2    0    1K  0 part
	└─sda5                 8:5    0 63.5G  0 part
	├─vgvagrant-root   253:0    0 62.6G  0 lvm   /
	└─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]
	sdb                    8:16   0  2.5G  0 disk
	├─sdb1                 8:17   0    2G  0 part
	│ └─md0                9:0    0    2G  0 raid1
	└─sdb2                 8:18   0  511M  0 part
	└─md1                9:1    0 1018M  0 raid0
		└─vg1-lvol0      253:2    0  100M  0 lvm
	sdc                    8:32   0  2.5G  0 disk
	├─sdc1                 8:33   0    2G  0 part
	│ └─md0                9:0    0    2G  0 raid1
	└─sdc2                 8:34   0  511M  0 part
	└─md1                9:1    0 1018M  0 raid0
		└─vg1-lvol0      253:2    0  100M  0 lvm
	vagrant@vagrant:~$

### 11. Создайте mkfs.ext4 ФС на получившемся LV.

	vagrant@vagrant:~$ sudo mkfs.ext4 /dev/vg1/lvol0
	mke2fs 1.45.5 (07-Jan-2020)
	Creating filesystem with 25600 4k blocks and 25600 inodes
	
	Allocating group tables: done
	Writing inode tables: done
	Creating journal (1024 blocks): done
	Writing superblocks and filesystem accounting information: done
	
	vagrant@vagrant:~$

### 12. Смонтируйте этот раздел в любую директорию, например, /tmp/new.

	vagrant@vagrant:~$ mkfs.ext4 /dev/vg1/lvol0
	mke2fs 1.45.5 (07-Jan-2020)
	Could not open /dev/vg1/lvol0: Permission denied
	vagrant@vagrant:~$ sudo mkfs.ext4 /dev/vg1/lvol0
	mke2fs 1.45.5 (07-Jan-2020)
	Creating filesystem with 25600 4k blocks and 25600 inodes
	
	Allocating group tables: done
	Writing inode tables: done
	Creating journal (1024 blocks): done
	Writing superblocks and filesystem accounting information: done
	
	vagrant@vagrant:~$ mkdir /tmp/new
	vagrant@vagrant:~$ ll /tmp/new
	total 8
	drwxrwxr-x  2 vagrant vagrant 4096 Feb  2 05:38 ./
	drwxrwxrwt 10 root    root    4096 Feb  2 05:38 ../
	vagrant@vagrant:~$ sudo mount /dev/vg1/lvol0 /tmp/new
	vagrant@vagrant:~$ lsblk
	NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
	sda                    8:0    0   64G  0 disk
	├─sda1                 8:1    0  512M  0 part  /boot/efi
	├─sda2                 8:2    0    1K  0 part
	└─sda5                 8:5    0 63.5G  0 part
	├─vgvagrant-root   253:0    0 62.6G  0 lvm   /
	└─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]
	sdb                    8:16   0  2.5G  0 disk
	├─sdb1                 8:17   0    2G  0 part
	│ └─md0                9:0    0    2G  0 raid1
	└─sdb2                 8:18   0  511M  0 part
	└─md1                9:1    0 1018M  0 raid0
		└─vg1-lvol0      253:2    0  100M  0 lvm   /tmp/new
	sdc                    8:32   0  2.5G  0 disk
	├─sdc1                 8:33   0    2G  0 part
	│ └─md0                9:0    0    2G  0 raid1
	└─sdc2                 8:34   0  511M  0 part
	└─md1                9:1    0 1018M  0 raid0
		└─vg1-lvol0      253:2    0  100M  0 lvm   /tmp/new
	vagrant@vagrant:~$
	
	
### 13. Поместите туда тестовый файл, например wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz.

	vagrant@vagrant:~$ wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz.
	/tmp/new/test.gz.: Permission denied
	vagrant@vagrant:~$ sudo wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz.
	--2022-02-02 05:39:38--  https://mirror.yandex.ru/ubuntu/ls-lR.gz
	Resolving mirror.yandex.ru (mirror.yandex.ru)... 213.180.204.183, 2a02:6b8::183
	Connecting to mirror.yandex.ru (mirror.yandex.ru)|213.180.204.183|:443... connected.
	HTTP request sent, awaiting response... 200 OK
	Length: 22133788 (21M) [application/octet-stream]
	Saving to: ‘/tmp/new/test.gz.’
	
	/tmp/new/test.gz.             100%[=================================================>]  21.11M  4.00MB/s    in 7.8s
	
	2022-02-02 05:39:46 (2.70 MB/s) - ‘/tmp/new/test.gz.’ saved [22133788/22133788]
	
	vagrant@vagrant:~$ ll /tmp/new
	total 21640
	drwxr-xr-x  3 root root     4096 Feb  2 05:39 ./
	drwxrwxrwt 10 root root     4096 Feb  2 05:38 ../
	drwx------  2 root root    16384 Feb  2 05:37 lost+found/
	-rw-r--r--  1 root root 22133788 Feb  2 04:36 test.gz.
	vagrant@vagrant:~$

### 14. Прикрепите вывод lsblk.

	vagrant@vagrant:~$ lsblk
	NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
	sda                    8:0    0   64G  0 disk
	├─sda1                 8:1    0  512M  0 part  /boot/efi
	├─sda2                 8:2    0    1K  0 part
	└─sda5                 8:5    0 63.5G  0 part
	├─vgvagrant-root   253:0    0 62.6G  0 lvm   /
	└─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]
	sdb                    8:16   0  2.5G  0 disk
	├─sdb1                 8:17   0    2G  0 part
	│ └─md0                9:0    0    2G  0 raid1
	└─sdb2                 8:18   0  511M  0 part
	└─md1                9:1    0 1018M  0 raid0
		└─vg1-lvol0      253:2    0  100M  0 lvm   /tmp/new
	sdc                    8:32   0  2.5G  0 disk
	├─sdc1                 8:33   0    2G  0 part
	│ └─md0                9:0    0    2G  0 raid1
	└─sdc2                 8:34   0  511M  0 part
	└─md1                9:1    0 1018M  0 raid0
		└─vg1-lvol0      253:2    0  100M  0 lvm   /tmp/new
	vagrant@vagrant:~$

### 15. Протестируйте целостность файла:
root@vagrant:~# gzip -t /tmp/new/test.gz
root@vagrant:~# echo $?
0

	vagrant@vagrant:~$ ll /tmp/new/test.gz.
	-rw-r--r-- 1 root root 22133788 Feb  2 04:36 /tmp/new/test.gz.
	vagrant@vagrant:~$ gzip -t /tmp/new/test.gz.
	vagrant@vagrant:~$ echo $?
	0
	vagrant@vagrant:~$

### 16. Используя pvmove, переместите содержимое PV с RAID0 на RAID1.

vagrant@vagrant:~$ sudo pvmove -n /dev/vg1/lvol0 /dev/md1 /dev/md0
  /dev/md1: Moved: 72.00%
  /dev/md1: Moved: 100.00%
vagrant@vagrant:~$

	vagrant@vagrant:~$ lsblk
	NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
	sda                    8:0    0   64G  0 disk
	├─sda1                 8:1    0  512M  0 part  /boot/efi
	├─sda2                 8:2    0    1K  0 part
	└─sda5                 8:5    0 63.5G  0 part
	├─vgvagrant-root   253:0    0 62.6G  0 lvm   /
	└─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]
	sdb                    8:16   0  2.5G  0 disk
	├─sdb1                 8:17   0    2G  0 part
	│ └─md0                9:0    0    2G  0 raid1
	│   └─vg1-lvol0      253:2    0  100M  0 lvm   /tmp/new
	└─sdb2                 8:18   0  511M  0 part
	└─md1                9:1    0 1018M  0 raid0
	sdc                    8:32   0  2.5G  0 disk
	├─sdc1                 8:33   0    2G  0 part
	│ └─md0                9:0    0    2G  0 raid1
	│   └─vg1-lvol0      253:2    0  100M  0 lvm   /tmp/new
	└─sdc2                 8:34   0  511M  0 part
	└─md1                9:1    0 1018M  0 raid0
	vagrant@vagrant:~$

### 17. Сделайте --fail на устройство в вашем RAID1 md.

	
	vagrant@vagrant:~$ sudo mdadm /dev/md0 --fail /dev/sdb1
	mdadm: set /dev/sdb1 faulty in /dev/md0
	vagrant@vagrant:~$
	
	vagrant@vagrant:~$ sudo mdadm -D /dev/md0
	/dev/md0:
			Version : 1.2
		Creation Time : Wed Feb  2 05:19:15 2022
			Raid Level : raid1
			Array Size : 2094080 (2045.00 MiB 2144.34 MB)
		Used Dev Size : 2094080 (2045.00 MiB 2144.34 MB)
		Raid Devices : 2
		Total Devices : 2
		Persistence : Superblock is persistent
	
		Update Time : Wed Feb  2 08:50:34 2022
				State : clean, degraded
		Active Devices : 1
	Working Devices : 1
		Failed Devices : 1
		Spare Devices : 0
	
	Consistency Policy : resync
	
				Name : vagrant:0  (local to host vagrant)
				UUID : c3bd0da9:abdad700:82e1a139:d96b5a13
				Events : 19
	
		Number   Major   Minor   RaidDevice State
		-       0        0        0      removed
		1       8       33        1      active sync   /dev/sdc1
	
		0       8       17        -      faulty   /dev/sdb1
	vagrant@vagrant:~$
	

### 18. Подтвердите выводом dmesg, что RAID1 работает в деградированном состоянии.

	vagrant@vagrant:~$ dmesg |grep md0
	[ 1473.174259] md/raid1:md0: not clean -- starting background reconstruction
	[ 1473.174261] md/raid1:md0: active with 2 out of 2 mirrors
	[ 1473.174285] md0: detected capacity change from 0 to 2144337920
	[ 1473.178706] md: resync of RAID array md0
	[ 1486.426427] md: md0: resync done.
	[ 6868.751948] md/raid1:md0: Disk failure on sdb1, disabling device.
				md/raid1:md0: Operation continuing on 1 devices.
	vagrant@vagrant:~$

### 19. Протестируйте целостность файла, несмотря на "сбойный" диск он должен продолжать быть доступен:
root@vagrant:~# gzip -t /tmp/new/test.gz
root@vagrant:~# echo $?
0

	vagrant@vagrant:~$ gzip -t /tmp/new/test.gz.
	vagrant@vagrant:~$ echo $?
	0
	vagrant@vagrant:~$

### 20. Погасите тестовый хост, vagrant destroy.

	vagrant@vagrant:~$ exit
	logout
	Connection to 127.0.0.1 closed.
	PS D:\devops-netology\vagrant project> vagrant destroy
		default: Are you sure you want to destroy the 'default' VM? [y/N] y
	==> default: Forcing shutdown of VM...
	==> default: Destroying VM and associated drives...
	PS D:\devops-netology\vagrant project>
	


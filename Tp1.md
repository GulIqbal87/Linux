TP 1 Linux 
 
 
        Préparation de la machine

            . Après avoir set up nos 2 machines virtuelles en leur donnant un accès internet via Nat et et un accés à un réseau local depuis Virtual Box, on utilise maintenant ssh pour effectuer les commandes ci-dessous.

            On donne un nom aux machines, 
            En ssh 10.101.1.11/24 :   [root@localhost pierre]# echo 'node1.tp1.b2' | tee /etc/hostname .
            En ssh 10.101.1.12/24 :   [root@localhost pierre]# echo 'node2.tp1.b2' | tee /etc/hostname .

            Après redémarrage, 
                [root@node1 pierre]# hostname
                node1.tp1.b2

                [root@node2 pierre]# hostname
                node2.tp1.b2


            Editer le fichier hosts, 
                [root@node1 pierre]# vim /etc/hosts 
                [root@node1 pierre]# cat /etc/hosts
                127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
                ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
                10.101.1.12 node2.tp1.b2



                [root@node2 pierre]# vim /etc/hosts
                [root@node2 pierre]# cat /etc/hosts
                127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
                ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
                10.101.1.11 node1.tp1.b2

            Ping les machines depuis leur noms, 
                [pierre@node1 ~]$ ping node2.tp1.b2
                PING node2.tp1.b2 (10.101.1.12) 56(84) bytes of data.
                64 bytes from node2.tp1.b2 (10.101.1.12): icmp_seq=1 ttl=64 time=1.96 ms
                64 bytes from node2.tp1.b2 (10.101.1.12): icmp_seq=2 ttl=64 time=1.57  ms
                64 bytes from node2.tp1.b2 (10.101.1.12): icmp_seq=3 ttl=64 time=1.30 ms
                64 bytes from node2.tp1.b2 (10.101.1.12): icmp_seq=4 ttl=64 time=0.520 ms
                64 bytes from node2.tp1.b2 (10.101.1.12): icmp_seq=5 ttl=64 time=1.37 ms
                64 bytes from node2.tp1.b2 (10.101.1.12): icmp_seq=6 ttl=64 time=1.39 ms
                --- node2.tp1.b2 ping statistics ---
                6 packets transmitted, 6 received, 0% packet loss, time 5020ms


                [pierre@node2 ~]$ ping node1.tp1.b2
                PING node1.tp1.b2 (10.101.1.11) 56(84) bytes of data.
                64 bytes from node1.tp1.b2 (10.101.1.11): icmp_seq=1 ttl=64 time=1.00 ms
                64 bytes from node1.tp1.b2 (10.101.1.11): icmp_seq=2 ttl=64 time=1.29 ms
                64 bytes from node1.tp1.b2 (10.101.1.11): icmp_seq=3 ttl=64 time=2.28 ms
                --- node1.tp1.b2 ping statistics ---
                3 packets transmitted, 3 received, 0% packet loss, time 2005ms
                rtt min/avg/max/mdev = 1.002/1.525/2.280/0.546 ms


            Ajout du serveur DNS en rajoutant la ligne DNS1=1.1.1.1 dans les fichiers :
                [root@node1 pierre]# vim /etc/sysconfig/network-scripts/ifcfg-enp0s3
                [root@node2 pierre]# vim /etc/sysconfig/network-scripts/ifcfg-enp0s3


                [pierre@node1 ~]$ dig ynov.com
                ;; Query time: 103 msec
                ;; SERVER: 1.1.1.1#53(1.1.1.1)

                [pierre@node2 ~]$ dig ynov.com
                ;; Query time: 103 msec
                ;; SERVER: 1.1.1.1#53(1.1.1.1)
                


        I / Utilisateurs 

            1 ) Création et configuration 
                [root@node1]# useradd user1 -m -s /bin/bash -u 2000
                [pierre@node1 home]$ ls
                pierre  user1

                [root@node2]# useradd user2 -m -s /bin/bash -u 2000
                [pierre@node2 home]$ ls
                pierre  user2

                [pierre@node1]$ cat /etc/passwd
                user1:x:2000:2000::/home/user1:/bin/bash

                [pierre@node2]# cat /etc/passwd
                user2:x:2000:2000::/home/user2:/bin/bash

                [root@node1 /]# groupadd admins
                [root@node2 /]# groupadd admins

                [root@node1 /]# visudo /etc/sudoers
                %admins ALL=(ALL)    ALL
                [root@node2 /]# visudo /etc/sudoers
                %admins ALL=(ALL)    ALL

                [root@node1 /]# usermod -aG admins user1
                [root@node1 /]# groups user1
                user1 : user1 admins

                [root@node2 /]# usermod -aG admins user2
                [root@node2 /]# groups user2
                user2 : user2 admins

                [pierre@node1 ~]$ sudo -l
                User pierre may run the following commands on node1:
                (ALL) NOPASSWD: ALL
                

                [pierre@node2 ~]$ sudo -l
                User pierre may run the following commands on node2:
                (ALL) ALL



            2 ) SSH    
                pierre@ubuntu-pierre:~$ ssh-keygen -t rsa -b 4096
                Generating public/private rsa key pair.
                Enter file in which to save the key (/home/pierre/.ssh/id_rsa): 
                Enter passphrase (empty for no passphrase): 
                Enter same passphrase again: 
                Your identification has been saved in /home/pierre/.ssh/id_rsa
                Your public key has been saved in /home/pierre/.ssh/id_rsa.pub
                The key fingerprint is:
                SHA256:ANF3VC4G1uL906jHhOm9Y4U+zri/VvQetIWSBgsLpcI pierre@ubuntu-pierre
                The key's randomart image is:
                +---[RSA 4096]----+
                |    oo .+o...    |
                |   . .oo+.+.     |
                |    E o+ *oo.. . |
                |     . .o.o.+ o..|
                |        S  = *..o|
                |          o * +o.|
                |         . * + ..|
                |          ooX   .|
                |          oB*=   |
                +----[SHA256]-----+


                pierre@ubuntu-pierre:~$ ssh-copy-id -i  /home/pierre/.ssh/id_rsa pierre@10.101.1.11
                /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/pierre/.ssh/id_rsa.pub"
                /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
                /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
                
                pierre@10.101.1.11's password: 

                Number of key(s) added: 1

                Now try logging into the machine, with:   "ssh 'pierre@10.101.1.11'"
                and check to make sure that only the key(s) you wanted were added.

                pierre@ubuntu-pierre:~$ ssh pierre@10.101.1.11

                pierre@ubuntu-pierre:~$ ssh-copy-id -i  /home/pierre/.ssh/id_rsa pierre@10.101.1.12
                /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/pierre/.ssh/id_rsa.pub"
                /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
                /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
                
                pierre@10.101.1.12's password: 

                Number of key(s) added: 1

                Now try logging into the machine, with:   "ssh 'pierre@10.101.1.12'"
                and check to make sure that only the key(s) you wanted were added.

                pierre@ubuntu-pierre:~$ ssh pierre@10.101.1.12
                Last login: Thu Nov 17 10:39:46 2022 from 10.101.1.1
                [pierre@node2 ~]$ 

        II / PArtitionnement

            1 ) Préparation de la VM

            2 ) Partitionnement 

                [pierre@node1 ~]$ lsblk
                
                NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
                sda           8:0    0    8G  0 disk 
                ├─sda1        8:1    0    1G  0 part /boot
                └─sda2        8:2    0    7G  0 part 
                ├─rl-root 253:0    0  6.2G  0 lvm  /
                └─rl-swap 253:1    0  820M  0 lvm  [SWAP]
                sdb           8:16   0    3G  0 disk 
                sdc           8:32   0    3G  0 disk 
                sr0          11:0    1 1024M  0 rom  
                
                
                [pierre@node1 ~]$ sudo pvcreate /dev/sdb
                
                Physical volume "/dev/sdb" successfully created.
                
                
                [pierre@node1 ~]$ sudo pvcreate /dev/sdc
                
                Physical volume "/dev/sdc" successfully created.
                
                
                [pierre@node1 ~]$ sudo pvs
                
                PV         VG Fmt  Attr PSize  PFree
                /dev/sda2  rl lvm2 a--  <7.00g    0 
                /dev/sdb      lvm2 ---   3.00g 3.00g
                /dev/sdc      lvm2 ---   3.00g 3.00g
                
                
                [pierre@node1 ~]$ sudo pvdisplay
                
                --- Physical volume ---
                PV Name               /dev/sda2
                VG Name               rl
                PV Size               <7.00 GiB / not usable 3.00 MiB
                Allocatable           yes (but full)
                PE Size               4.00 MiB
                Total PE              1791
                Free PE               0
                Allocated PE          1791
                PV UUID               3znvVo-S6tG-cMIz-bGY4-C0mK-86Uu-NLE9SA
                
                "/dev/sdb" is a new physical volume of "3.00 GiB"
                --- NEW Physical volume ---
                PV Name               /dev/sdb
                VG Name               
                PV Size               3.00 GiB
                Allocatable           NO
                PE Size               0   
                Total PE              0
                Free PE               0
                Allocated PE          0
                PV UUID               n5eAkz-ojLH-0kRB-JuNj-CRMj-ktqi-dUST8k
                
                "/dev/sdc" is a new physical volume of "3.00 GiB"
                --- NEW Physical volume ---
                PV Name               /dev/sdc
                VG Name               
                PV Size               3.00 GiB
                Allocatable           NO
                PE Size               0   
                Total PE              0
                Free PE               0
                Allocated PE          0
                PV UUID               QHpxct-Q39o-PSTa-HRaU-9rIc-scid-Emayya


                [pierre@node1 ~]$ sudo vgcreate VG_name /dev/sdb /dev/sdc
                
                Volume group "VG_name" successfully created
                
                
                [pierre@node1 ~]$ sudo vgs
                
                VG      #PV #LV #SN Attr   VSize  VFree
                VG_name   2   0   0 wz--n-  5.99g 5.99g
                rl        1   2   0 wz--n- <7.00g    0 
                
                
                [pierre@node1 ~]$ sudo vgdisplay
                
                --- Volume group ---
                VG Name               VG_name
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
                VG Size               5.99 GiB
                PE Size               4.00 MiB
                Total PE              1534
                Alloc PE / Size       0 / 0   
                Free  PE / Size       1534 / 5.99 GiB
                VG UUID               5FSxIY-OplZ-lWL7-Bj6L-qDMc-sEOU-Wt8KAU
                
                --- Volume group ---
                VG Name               rl
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
                VG Size               <7.00 GiB
                PE Size               4.00 MiB
                Total PE              1791
                Alloc PE / Size       1791 / <7.00 GiB
                Free  PE / Size       0 / 0   
                VG UUID               BNaOaS-fddK-b2lC-Fltd-cw19-2PCz-nqCGhF


                [pierre@node1 dev]$ sudo mkfs -t ext4 /dev/VG_name/data
                
                mke2fs 1.46.5 (30-Dec-2021)
                Creating filesystem with 262144 4k blocks and 65536 inodes
                Filesystem UUID: cd381f16-f6f7-467c-baba-9383ba6c34dd
                Superblock backups stored on blocks: 
                        32768, 98304, 163840, 229376

                Allocating group tables: done                            
                Writing inode tables: done                            
                Creating journal (8192 blocks): done
                Writing superblocks and filesystem accounting information: done

                [pierre@node1 dev]$ sudo mkfs -t ext4 /dev/VG_name/data1
                
                mke2fs 1.46.5 (30-Dec-2021)
                Creating filesystem with 262144 4k blocks and 65536 inodes
                Filesystem UUID: 818f0e47-6505-4dce-b3fd-e257ec09fe40
                Superblock backups stored on blocks: 
                        32768, 98304, 163840, 229376

                Allocating group tables: done                            
                Writing inode tables: done                            
                Creating journal (8192 blocks): done
                Writing superblocks and filesystem accounting information: done

                [pierre@node1 dev]$ sudo mkfs -t ext4 /dev/VG_name/data2
                
                mke2fs 1.46.5 (30-Dec-2021)
                Creating filesystem with 262144 4k blocks and 65536 inodes
                Filesystem UUID: 8ac82681-4fbe-4eed-b562-419c38241d9d
                Superblock backups stored on blocks: 
                        32768, 98304, 163840, 229376

                Allocating group tables: done                            
                Writing inode tables: done                            
                Creating journal (8192 blocks): done
                Writing superblocks and filesystem accounting information: done


                [pierre@node1 /]$ sudo mkdir /mnt/part1
                [pierre@node1 /]$ sudo mount /dev/VG_name/data /mnt/part1
                [pierre@node1 /]$ mount
                [pierre@node1 /]$ df -h
                Filesystem                Size  Used Avail Use% Mounted on
                devtmpfs                  210M     0  210M   0% /dev
                tmpfs                     229M     0  229M   0% /dev/shm
                tmpfs                      92M  3.3M   89M   4% /run
                /dev/mapper/rl-root       6.2G 1000M  5.3G  16% /
                /dev/sda1                1014M  166M  849M  17% /boot
                tmpfs                      46M     0   46M   0% /run/user/1000
                /dev/mapper/VG_name-data  974M   24K  907M   1% /mnt/part1

                [pierre@node1 /]$ sudo mkdir /mnt/part2
                [pierre@node1 /]$ sudo mount /dev/VG_name/data1 /mnt/part2
                [pierre@node1 /]$ mount
                [pierre@node1 /]$ df -h
                Filesystem                 Size  Used Avail Use% Mounted on
                devtmpfs                   210M     0  210M   0% /dev
                tmpfs                      229M     0  229M   0% /dev/shm
                tmpfs                       92M  3.3M   89M   4% /run
                /dev/mapper/rl-root        6.2G 1000M  5.3G  16% /
                /dev/sda1                 1014M  166M  849M  17% /boot
                tmpfs                       46M     0   46M   0% /run/user/1000
                /dev/mapper/VG_name-data   974M   24K  907M   1% /mnt/part1
                /dev/mapper/VG_name-data1  974M   24K  907M   1% /mnt/part2

                [pierre@node1 /]$ sudo mkdir /mnt/part3
                [pierre@node1 /]$ sudo mount /dev/VG_name/data2 /mnt/part3
                [pierre@node1 /]$ mount
                [pierre@node1 /]$ df -h
                Filesystem                 Size  Used Avail Use% Mounted on
                devtmpfs                   210M     0  210M   0% /dev
                tmpfs                      229M     0  229M   0% /dev/shm
                tmpfs                       92M  3.3M   89M   4% /run
                /dev/mapper/rl-root        6.2G 1000M  5.3G  16% /
                /dev/sda1                 1014M  166M  849M  17% /boot
                tmpfs                       46M     0   46M   0% /run/user/1000
                /dev/mapper/VG_name-data   974M   24K  907M   1% /mnt/part1
                /dev/mapper/VG_name-data1  974M   24K  907M   1% /mnt/part2
                /dev/mapper/VG_name-data2  974M   24K  907M   1% /mnt/part3


                [pierre@node1 /]$ sudo vim /etc/fstab
                [pierre@node1 /]$ sudo umount /mnt/part1
                [pierre@node1 /]$ sudo mount -av
                /                        : ignored
                /boot                    : already mounted
                none                     : ignored
                mount: /mnt/part1 does not contain SELinux labels.
                    You just mounted a file system that supports labels which does not
                    contain labels, onto an SELinux box. It is likely that confined
                    applications will generate AVC messages and not be allowed access to
                    this file system.  For more details see restorecon(8) and mount(8).
                /mnt/part1               : successfully mounted
                /mnt/part2               : already mounted
                /mnt/part3               : already mounted


        III / Gestion de services  
            
            1 ) Intéraction avec un service existant

                [pierre@node1 ~]$ sudo systemctl is-active firewalld
                
                active
                
                [pierre@node1 ~]$ sudo systemctl is-enabled firewalld
                
                enabled

            2 ) Création de service 

                A. Unité Simpliste

                    [pierre@node1 ~]$ sudo vim /etc/systemd/system/web.service
                    
                    [pierre@node1 ~]$ sudo firewall-cmd --add-port=8888/tcp --permanent
                    success
                    
                    [pierre@node1 ~]$ sudo systemctl daemon-reload
                    
                    [pierre@node1 ~]$ sudo systemctl status web
                    ○ web.service - Very simple web service
                        Loaded: loaded (/etc/systemd/system/web.service; disabled; vendor preset: disabled)
                        Active: inactive (dead)
                    
                    [pierre@node1 ~]$ sudo systemctl start web
                    
                    [pierre@node1 ~]$ sudo systemctl enable web
                    Created symlink /etc/systemd/system/multi-user.target.wants/web.service → /etc/systemd/system/web.service.

                    [pierre@node1 ~]$ curl localhost:8888
                    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
                    <html>
                    <head>
                    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
                    <title>Directory listing for /</title>
                    </head>
                    <body>
                    <h1>Directory listing for /</h1>
                    <hr>
                    <ul>
                    <li><a href="afs/">afs/</a></li>
                    <li><a href="bin/">bin@</a></li>
                    <li><a href="boot/">boot/</a></li>
                    <li><a href="dev/">dev/</a></li>
                    <li><a href="etc/">etc/</a></li>
                    <li><a href="home/">home/</a></li>
                    <li><a href="lib/">lib@</a></li>
                    <li><a href="lib64/">lib64@</a></li>
                    <li><a href="media/">media/</a></li>
                    <li><a href="mnt/">mnt/</a></li>
                    <li><a href="opt/">opt/</a></li>
                    <li><a href="proc/">proc/</a></li>
                    <li><a href="root/">root/</a></li>
                    <li><a href="run/">run/</a></li>
                    <li><a href="sbin/">sbin@</a></li>
                    <li><a href="srv/">srv/</a></li>
                    <li><a href="sys/">sys/</a></li>
                    <li><a href="tmp/">tmp/</a></li>
                    <li><a href="usr/">usr/</a></li>
                    <li><a href="var/">var/</a></li>
                    </ul>
                    <hr>
                    </body>
                    </html>

                    [pierre@node2 ~]$ curl 10.101.1.11:8888
                    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
                    <html>
                    <head>
                    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
                    <title>Directory listing for /</title>
                    </head>
                    <body>
                    <h1>Directory listing for /</h1>
                    <hr>
                    <ul>
                    <li><a href="afs/">afs/</a></li>
                    <li><a href="bin/">bin@</a></li>
                    <li><a href="boot/">boot/</a></li>
                    <li><a href="dev/">dev/</a></li>
                    <li><a href="etc/">etc/</a></li>
                    <li><a href="home/">home/</a></li>
                    <li><a href="lib/">lib@</a></li>
                    <li><a href="lib64/">lib64@</a></li>
                    <li><a href="media/">media/</a></li>
                    <li><a href="mnt/">mnt/</a></li>
                    <li><a href="opt/">opt/</a></li>
                    <li><a href="proc/">proc/</a></li>
                    <li><a href="root/">root/</a></li>
                    <li><a href="run/">run/</a></li>
                    <li><a href="sbin/">sbin@</a></li>
                    <li><a href="srv/">srv/</a></li>
                    <li><a href="sys/">sys/</a></li>
                    <li><a href="tmp/">tmp/</a></li>
                    <li><a href="usr/">usr/</a></li>
                    <li><a href="var/">var/</a></li>
                    </ul>
                    <hr>
                    </body>
                    </html>




                B. Modification de l'unité

                    [pierre@node1 /]$ sudo useradd web
                    [pierre@node1 /]$ sudo mkdir /var/www/
                    [pierre@node1 /]$ sudo mkdir /var/www/meow/
                    [pierre@node1 /]$ sudo touch /var/www/meow/toto

                    [pierre@node1 /]$ ls -al /var/www/meow/toto
                    -rw-r--r--. 1 root root 0 Nov 17 12:57 /var/www/meow/toto
                    
                    [pierre@node1 /]$ sudo vim /etc/systemd/system/web.service
                    [Unit]
                    Description=Very simple web service

                    [Service]
                    ExecStart=/usr/bin/python3 -m http.server 8888
                    User=web
                    WorkingDirecetory=/var/www/meow
                    [Install]
                    WantedBy=multi-user.target



                    



































                    


                    

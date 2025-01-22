# TP4 : Vers une maîtrise des OS Linux

## I. Partitionnement

🌞 Faites une install manuelle de Rocky Linux


```
[hugo@localhost ~]$ lsblk
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda           8:0    0   40G  0 disk
├─sda1        8:1    0    2G  0 part /boot
└─sda2        8:2    0   21G  0 part
  ├─rl-root 253:0    0   10G  0 lvm  /
  ├─rl-swap 253:1    0    1G  0 lvm  [SWAP]
  ├─rl-var  253:2    0    5G  0 lvm  /var
  └─rl-home 253:3    0    5G  0 lvm  /home
sr0          11:0    1 1024M  0 rom
```

```
[hugo@localhost ~]$ df -h
Filesystem           Size  Used Avail Use% Mounted on
devtmpfs             4.0M     0  4.0M   0% /dev
tmpfs                888M     0  888M   0% /dev/shm
tmpfs                356M  5.0M  351M   2% /run
/dev/mapper/rl-root  9.8G  1.1G  8.3G  12% /
/dev/mapper/rl-home  4.9G  4.6G     0 100% /home
/dev/mapper/rl-var   4.9G  115M  4.5G   3% /var
/dev/sda1            2.0G  232M  1.8G  12% /boot
tmpfs                178M     0  178M   0% /run/user/1000
```

```
[hugo@localhost ~]$ sudo pvs
[sudo] password for hugo:
  PV         VG Fmt  Attr PSize  PFree
  /dev/sda2  rl lvm2 a--  21.00g 4.00m
```

```
[hugo@localhost ~]$ sudo vgs
  VG #PV #LV #SN Attr   VSize  VFree
  rl   1   4   0 wz--n- 21.00g 4.00m
```

```
[hugo@localhost ~]$ sudo lvs
  LV   VG Attr       LSize  Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  home rl -wi-ao----  5.00g
  root rl -wi-ao---- 10.00g
  swap rl -wi-ao----  1.00g
  var  rl -wi-ao----  5.00g
```

## 2. Scénario remplissage de partition

🌞 Remplissez votre partition /home

🌞 Constater que la partition est pleine

```
[hugo@localhost ~]$ dd if=/dev/zero of=/home/hugo/bigfile bs=4M count=5000
dd: error writing '/home/hugo/bigfile': No space left on device
1171+0 records in
1170+0 records out
4911116288 bytes (4.9 GB, 4.6 GiB) copied, 9.67936 s, 507 MB/s
[hugo@localhost ~]$ df -h
Filesystem           Size  Used Avail Use% Mounted on
devtmpfs             4.0M     0  4.0M   0% /dev
tmpfs                888M     0  888M   0% /dev/shm
tmpfs                356M  5.0M  351M   2% /run
/dev/mapper/rl-root  9.8G  1.1G  8.3G  12% /
/dev/mapper/rl-home  4.9G  4.6G     0 100% /home
/dev/mapper/rl-var   4.9G  115M  4.5G   3% /var
/dev/sda1            2.0G  232M  1.8G  12% /boot
tmpfs                178M     0  178M   0% /run/user/1000
```

🌞 Agrandir la partition

```
[hugo@localhost ~]$ sudo lvextend -l +100%FREE /dev/mapper/rl-home
  Size of logical volume rl/home changed from 15.00 GiB (3840 extents) to 20.99 GiB (5374 extents).
  Logical volume rl/home successfully resized.
```

```
[hugo@localhost ~]$ sudo resize2fs /dev/mapper/rl-home
resize2fs 1.46.5 (30-Dec-2021)
Filesystem at /dev/mapper/rl-home is mounted on /home; on-line resizing required
old_desc_blocks = 1, new_desc_blocks = 3
The filesystem on /dev/mapper/rl-home is now 5502976 (4k) blocks long.

[hugo@localhost ~]$ df -h
Filesystem           Size  Used Avail Use% Mounted on
devtmpfs             4.0M     0  4.0M   0% /dev
tmpfs                888M     0  888M   0% /dev/shm
tmpfs                356M  5.0M  351M   2% /run
/dev/mapper/rl-root  9.8G  1.1G  8.3G  12% /
/dev/mapper/rl-home   21G  4.6G   16G  24% /home
/dev/mapper/rl-var   4.9G  115M  4.5G   3% /var
/dev/sda1            2.0G  232M  1.8G  12% /boot
tmpfs                178M     0  178M   0% /run/user/1000
```

🌞 Remplissez votre partition /home

🌞 Utiliser ce nouveau disque pour étendre la partition /home de 40G

```
[hugo@localhost ~]$ sudo pvcreate /dev/sdb
[sudo] password for hugo:
  Physical volume "/dev/sdb" successfully created.
```

```
[hugo@localhost ~]$ sudo vgextend rl /dev/sdb
[sudo] password for hugo:
  Volume group "rl" successfully extended
```

```
[hugo@localhost ~]$ sudo lvextend -l +100%FREE /dev/mapper/rl-home
  Size of logical volume rl/home changed from 20.99 GiB (5374 extents) to <60.99 GiB (15613 extents).
  Logical volume rl/home successfully resized.
```

```
[hugo@localhost ~]$ sudo resize2fs /dev/mapper/rl-home
resize2fs 1.46.5 (30-Dec-2021)
Filesystem at /dev/mapper/rl-home is mounted on /home; on-line resizing required
old_desc_blocks = 3, new_desc_blocks = 8
The filesystem on /dev/mapper/rl-home is now 15987712 (4k) blocks long.
```

```
[hugo@localhost ~]$ df -h /home
Filesystem           Size  Used Avail Use% Mounted on
/dev/mapper/rl-home   60G   20G   38G  35% /home
```

## II. Gestion de users


🌞 Gestion basique de users


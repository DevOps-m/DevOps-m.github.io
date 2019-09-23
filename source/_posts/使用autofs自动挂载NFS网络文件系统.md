---
title: 使用autofs自动挂载NFS网络文件系统
date: 2019-09-23 15:13:00
top: false
cover: false
password:
toc: true
mathjax: false
summary:
tags:
- Linux
categories:
- Linux
---

#### 系统信息：

系统版本 | 内核版本 | NFS版本
---|---|---
CentOS Linux release 7.6.1810 (Core) | 3.10.0-957.el7.x86_64 | 1.3.0

> PS：这里不介绍部署NFS网络共享文件系统内容。

#### 部署过程：

> 参考文献：https://www.linuxtechi.com/automount-nfs-share-in-linux-using-autofs/

1. 安装autofs软件：
```
root@linux:~# yum install -y autofs
```

2. 向/etc/auto.master内增加内容：
```
root@linux:~# echo "/ /etc/auto.fs" >> /etc/auto.master
```

3. 创建/etc/auto.fs，如存在则直接添加以下内容即可：
```
mnt -fstype=nfs,vers=4,minorversion=0,noatime,nodiratime,nosuid,noexec,nodev,ro,bg,soft,_netdev nfs.linux.com:/data/renderdata
```

4. 将autofs服务设置开机自启并启动：
```
root@linux:~# systemctl enable autofs.service
root@linux:~# systemctl start autofs.service
root@linux:~# systemctl status autofs.service
```

5. 测试autofs部署情况：
```
ops@linux:~$ df -h
Filesystem             Size  Used Avail Use% Mounted on
/dev/mapper/vg01-root  200G  4.0G  196G   2% /
devtmpfs                63G     0   63G   0% /dev
tmpfs                   63G     0   63G   0% /dev/shm
tmpfs                   63G   11M   63G   1% /run
tmpfs                   63G     0   63G   0% /sys/fs/cgroup
/dev/sda2              497M  132M  365M  27% /boot
/dev/sda1              500M   12M  489M   3% /boot/efi
/dev/mapper/vg01-data  3.1T   38M  3.1T   1% /data
tmpfs                   13G     0   13G   0% /run/user/1000
ops@linux:~$

ops@linux:~$ ls /mnt/
exportgeometry  modelvrscene  renderingvrscene  scripts
ops@linux:~$ 

ops@linux:~$ df -h
Filesystem                          Size  Used Avail Use% Mounted on
/dev/mapper/vg01-root               200G  4.0G  196G   2% /
devtmpfs                             63G     0   63G   0% /dev
tmpfs                                63G     0   63G   0% /dev/shm
tmpfs                                63G   11M   63G   1% /run
tmpfs                                63G     0   63G   0% /sys/fs/cgroup
nfs.linux.com:/data/renderdata        4.0T  1.2T  2.9T  29% /mnt
/dev/sda2                           497M  132M  365M  27% /boot
/dev/sda1                           500M   12M  489M   3% /boot/efi
/dev/mapper/vg01-data               3.1T   38M  3.1T   1% /data
tmpfs                                13G     0   13G   0% /run/user/1000
ops@linux:~$
```
> PS：可以看到测试过程如上所示。
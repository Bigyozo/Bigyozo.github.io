# samba 文件服务器搭建

### 树莓派环境配置 samba 文件服务器

下载 samba 服务并进行配置

```
apt install samba samba-common-bin
nano /etc/samba/smb.conf
```

在 smb.conf 末尾添加如下内容

```
[MyNAS]
  valid users = pi, root
  path = /media/root
  browseable = yes
  writable = yes
  create mask = 0644
  directory mask = 0755
```

设置用户密码及开机自启动

```
smbpasswd -a pi
systemctl start smbd
systemctl enable smbd
```

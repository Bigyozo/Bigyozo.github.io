# samba ファイルサーバー構築

### ラズベリーパイ環境で samba ファイルサーバーの構築方法

samba をインストールし、構成ファイルを編集

```shell
apt install samba samba-common-bin
nano /etc/samba/smb.conf
```

smb.conf の末尾に以下の内容を追加

```
[MyNAS]
  valid users = pi, root
  path = /media/root
  browseable = yes
  writable = yes
  create mask = 0644
  directory mask = 0755
```

ユーザーパスワードと起動時の自動起動を設置

```shell
smbpasswd -a pi
systemctl start smbd
systemctl enable smbd
```

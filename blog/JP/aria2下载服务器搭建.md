# aria2 ダウンロードサーバー構築

### ラズベリーパイ環境で Aria2 というダウンロードサーバーおよび AriaNg というフロントエンドサービスを構築する方法

aria2 と nginx をダウンロード

```shell
apt install aria2
apt install nginx
```

nginx のプロジェクトディレクトリを開き、AriaNg の圧縮ファイルをダウンロードして解凍

```shell
cd /var/www/html/
wget https://github.com/mayswind/AriaNg/releases/download/1.2.4/AriaNg-1.2.4-AllInOne.zip
unzip AriaNg-1.2.4-AllInOne.zip
```

フロントエンドサービスの ariaNg を起動

```shell
systemctl start nginx
systemctl enable nginx
```

バックエンドサービスの aria2 を起動して、パスワードを設定

```shell
aria2c --enable-rpc --rpc-listen-all --rpc-secret yourPassword
```

ariaNg の画面で IP を設定し、先程設定したパスワードを入力

![ariaNg](../../res/ariaNg.png)

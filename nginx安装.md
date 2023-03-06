# nginx 安装

解压缩下载好的安装包

```shell
tar -zxvf nginx-1.22.1.tar.gz
```

执行./configure 时发现缺少 PCRE 库，下载依赖库

```shell
apt-get install libpcre3 libpcre3-dev
```

重新执行./configure

```shell
./configure --prefix=/usr/local/nginx
```

编译

```shell
make
```

安装

```shell
make install
```

在目录/usr/local/nginx 下安装完成
![nginx](res/nginx.png)

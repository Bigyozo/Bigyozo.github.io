# docker 环境容器部署

日常使用的各种 docker 镜像的容器部署指令。

- arm64v8 的镜像为树莓派环境部署使用。
- windows 子系统下的 docker 容器挂载目录为浏览器中输入

```shell
//wsl$/docker-desktop-data/version-pack-data/community/docker/volumes
```

### consul

```shell
docker run -d --name consulserver1 -p 8500:8500 --restart=always  arm64v8/consul agent -server -bootstrap-expect=1  -client 0.0.0.0 -ui
```

### portainer

```shell
docker run -d -p 9000:9000 --name portainer --restart always \
-v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
```

### 网心云 wxedge

```shell
docker run -d --name=wxedge --privileged --net=host --tmpfs /run  --tmpfs /tmp \
  -v /media/pi/SX5Pro/wxedge_storage:/storage:rw onething1/wxedge
```

### elasticsearch

```shell
echo "http.host: 0.0.0.0" >> /docker_data/elasticsearch/config/elasticsearch.yml

docker run --name es -p 9200:9200 -p 9300:9300  -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms512m -Xmx512m" \
-v /docker_data/elasticsearch/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml \
-v elasticsearch_data:/usr/share/elasticsearch/data \
-v /docker_data/elasticsearch/plugins:/usr/share/elasticsearch/plugins -d arm64v8/elasticsearch:7.11.1
```

### postwoman

```shell
docker run -p 3000:3000 liyasthomas/postwoman:latest
```

### mysql 主从复制

```shell
docker run --name mysql-master --privileged=true -v mysql-master-data:/var/lib/mysql -p 3306:3306 \
-e MYSQL_ROOT_PASSWORD=root -d mysql/mysql-server

docker run --name mysql-slave --privileged=true -v mysql-slave-data:/var/lib/mysql -p 3307:3306 \
--link mysql-master:master -e MYSQL_ROOT_PASSWORD=root -d mysql/mysql-server

echo server-id = 2 >> /etc/my.cnf
```

登录 master 容器创建用户并授权(mysql8.0 不支持同时创建用户并授权),查询 bin 日志的 master_log_file 和 master_log_pos 值,进入 slave 容器开启主从复制

```sql
create user 'slave'@'%' identified by '123456';

GRANT REPLICATION SLAVE ON *.* to 'slave'@'%' ;

show master status\G;

change master to master_host='master', master_user='slave', master_password='123456', master_port=3306, \
master_log_file='binlog.000002', master_log_pos=5884, master_connect_retry=30;

start slave;

show slave status\G;
```

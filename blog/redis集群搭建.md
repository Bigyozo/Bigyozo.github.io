# Docker环境下搭建Redis集群
对redis.conf文件进行如下修改，文件重命名为redis-cluster.tmpl
```
# bind 127.0.0.1
protected-mode no
port ${PORT}
daemonize no
dir /data/redis
appendonly yes
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 15000
```
创建docker内部网络
```
docker network create redis-cluster-net
```
创建 master 和 slave 文件夹并生成配置文件，用于存放配置文件redis.conf及redis数据
```
for port in `seq 7000 7005`; do
    ms="master"
    if [ $port -ge 7003 ]; then
        ms="slave"
    fi
    mkdir -p ./$ms/$port/ && mkdir -p ./$ms/$port/data \
    && PORT=$port envsubst < ./redis-cluster.tmpl > ./$ms/$port/redis.conf;
done
```
运行docker实例
```
for port in `seq 7000 7005`; do
    ms="master"
    if [ $port -ge 7003 ]; then
        ms="slave"
    fi
    docker run -d -p $port:$port -p 1$port:1$port \
    -v $PWD/$ms/$port/redis.conf:/data/redis.conf \
    -v $PWD/$ms/$port/data:/data/redis \
    --restart always --name redis-$ms-$port --net redis-cluster-net \
    redis redis-server /data/redis.conf;
done
```
获取创建的redis容器对应的ip及端口
```
matches=""
for port in `seq 7000 7005`; do
    ms="master"
    if [ $port -ge 7003 ]; then
        ms="slave"
    fi
    matches=$matches$(docker inspect --format '{{(index .NetworkSettings.Networks "redis-cluster-net").IPAddress}}' "redis-$ms-${port}"):${port}" ";
done
echo $matches
```
进入创建的redis实例，启动集群同步
```
redis-cli --cluster create $matches --cluster-replicas 1
```
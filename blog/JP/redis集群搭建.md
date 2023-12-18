# redis クラスター構築

ファイル redis.conf を以下のように修正し，ファイル名を redis-cluster.tmpl に設定

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

docker ネットワークを作成

```
docker network create redis-cluster-net
```

構成ファイル redis.conf および redis のデータを保存するためのフォルダを作成

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

docker コンテナーを起動

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

作成した redis コンテナに対応する IP とポートを取得

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

作成した redis インスタンスに入り、クラスタ同期を開始

```
redis-cli --cluster create $matches --cluster-replicas 1
```

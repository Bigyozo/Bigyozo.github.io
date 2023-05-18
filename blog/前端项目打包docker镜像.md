# 前端项目打包 docker 镜像

1. 创建 dockerFile

- FROM: 初始镜像
- SHELL: 使用脚本类型，如果不设置会导致之后无法进入容器
- WORKDIR: 工作目录，会被自动创建
- ENV: 环境变量
- COPY: 将当前文件复制进入镜像
- RUN: 执行命令
- EXPOSE: 暴露端口号
- ENTRYPOINT: 启动入口，对应项目的启动脚本

```dockerFile
FROM node:lts-alpine
SHELL ["/bin/sh","-c"]
ENV API_IP 172.17.0.3
ENV API_PORT 3000
ENV PORT 8800
WORKDIR /app
COPY ./www /app/www/
COPY ./server.js /app/
COPY ./package.json /app/
RUN npm install
EXPOSE $PORT
ENTRYPOINT ["node","server.js"]
```

2. 生成 docker 镜像

```shell
docker build -t imageName .
docker tag imageId dockerUserId/imageName:latest
```

3. 登陆 docker 用户并上传镜像

```shell
docker login -u dockerUserId
docker image push dockerUserId/imageName:latest
```

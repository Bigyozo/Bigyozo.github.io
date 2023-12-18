# docker イメージ作成

1. dockerFile の作成

- FROM: イニシャルイメージ
- SHELL: スクリプトのタイプを指定する。設定しないと、後でコンテナに入れなくなる
- WORKDIR: 作業ディレクトリ、自動的に作成される
- ENV: 環境変数
- COPY: 指定されたファイルはイメージにコピーする
- RUN: 実行コマンド
- EXPOSE: 公開のポート番号
- ENTRYPOINT: 起動エントリーポイント、プロジェクトの起動スクリプトになる

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

1. docker イメージを作成

```shell
docker build -t imageName .
docker tag imageId dockerUserId/imageName:latest
```

1. docker のアカウントを登録し、イメージをアップロード

```shell
docker login -u dockerUserId
docker image push dockerUserId/imageName:latest
```

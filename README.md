# Project structure
```
. PROJECT_ROOT
└── front
    ├── node_modules/
    ├── public/
    ├── src/
    ├── Dockerfile
    ├── docker-compose.yml
    ├── package-lock.json
    ├── package.json
    └── tsconfig.json 
```

# 作成手順
### 1.npx create-react-app . --template typescript
```:shell
$ mkdir front && cd front
$ npx create-react-app . --template typescript // 今いるフォルダ内(front)にcreate-react-appのビルド
```

### 2.Dockerfileを作成
```Dockerfile:Dockerfile
# Node.js Docker image Linux 軽量ディストリビューション
FROM node:18-alpine
# ./(PROJECT_ROOT以下)をイメージのファイルシステム上の/frontにコピー
COPY ./ /front
# コマンドを実行する場所として使う作業ディレクトリ
WORKDIR /front
# パッケージをインストール
RUN npm install
#　localhost:3000で起動
CMD npm start
```
### 3.docker-compose.ymlを作成
```yml:docker-compose.yml
version: "3.7"
services:
  front:
    container_name: front
    build:
      context: .
      dockerfile: Dockerfile
    volumes:
    # ./を/frontにバインドマウント
      - type: bind
        source: ./
        target: /front
    working_dir: /front
    command: npm start
    # 標準入出力先のデバイス コンテナが正常終了するのを防ぐ コンテナを持続させる
    tty: true
    ports:
    # コンテナの3000ポートをホストマシンの3000ポートに割り当てる
    # ホスト側：コンテナ側
      - 3000:3000
```
## コンテナを起動
```:sehll
$ docker compose up
```
## localhost:3000でReactが起動しているか確認
[http://localhost:3000](http://localhost:3000)

## コンテナに入る
`コンテナは起動したまま`
```
$ docker container exec -it front sh
```

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

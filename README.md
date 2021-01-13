# 作成手順
- 手順2,3 以外はテンプレートで作成済
## 1. express-generatorでひな形作成
- 下記を実行
```
# インストール用のディレクトリ作成
mkdir generator
cd generator

# express-generatorをインストール
npm install express-generator

# 雛形作成
./node_modules/.bin/express --view=ejs ../myapp

# インストール用に作成したファイルを削除
rm -rf generator
```
## 2. envファイルを作成
- app.envを作成し以下を定義
```
MYSQL_SERVER=mysql
```
- mysql/mysql.envを作成し以下を定義
```
MYSQL_ROOT_HOST=%
MYSQL_ROOT_PASSWORD=rootパスワード
MYSQL_USER=mysqlユーザ名
MYSQL_PASSWORD=mysqlユーザパスワード
MYSQL_DATABASE=データベース名
```
## 3. dockerイメージのビルドとコンテナの作成
- docker-copose.ymlの `image` と `container_name` を変更
- 下記を実行
```
docker-compose up(-- build)
```

## 4. express-ejs-layoutsを追加
- 下記を実行
```
docker-compose exec app npm install --save express-ejs-layouts
```

## 5. nodemonを追加
- 下記を実行
```
docker-compose exec  app npm install --save nodemon
```
- `pakage.json`のscriptsに書きを追加
```
"watch": "nodemon ./bin/www"
```
- docker-compose.ymlのcommandを下記に変更
```
command: [sh, -c, npm install && npm run watch]
```
- docker-composeを再起動

## 6. sequelizeを追加
- 必要パッケージをインストール
```
docker-compose exec app npm install --save sequelize
docker-compose exec app npm install --save mysql2
docker-compose exec app npm install --save sequelize-cli
```

## 7. sequelizeの設定
- 下記コマンドを事項
```
# Sequelizeの初期化
docker-compose exec app npx sequelize init
```
- `myqpp/config/config.json` に下記を記述
```
"username":DBユーザ名,
"password": DBユーザパスワード,
"database": データベース名,
"host": MYSQL_SERVER,
"dialect": "mysql"
```

### 8. マイグレーション
```
docker-compose exec app node_modules/.bin/sequelize db:migrate
```
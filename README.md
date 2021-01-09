# 作成手順
## 1. express-generatorでひな形作成
```
# インストール用のディレクトリ作成(ここまでテンプレートで作成済)
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
mysqlを定義
```
- mysql/mysql.envを作成し以下を定義
```
ホスト名
rootパスワード
mysqlユーザ名
mysqlユーザパスワード
データベース名
```
## 3. dockerイメージのビルドとコンテナの作成
```
docker-compose up --build
```

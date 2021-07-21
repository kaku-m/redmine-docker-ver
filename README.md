# redmine-docker-ver

## 動作確認手順
```
$ git clone https://github.com/kaku-m/redmine-docker-ver.git
$ cd redmine-docker-ver
$ docker-compose up # -dオプションでバックグラウンドで起動できます
```
http://localhost:8080/ にアクセス　※ホストは環境に合わせてください  
画面右上のログインを押下  
ログインIDとパスワードに「admin」を入力してログイン  
パスワード変更を行い適用  
画面左上の管理を押下  
デフォルト設定をロードを押下  
画面左上のプロジェクトを押下  
新しいプロジェクトを作成する  
チケットタブを押下  
新しいチケットを作成する　※以下の手順を実施する場合、ファイルも添付してください  
以下のバックアップ手順を実施  
チケットタブを押下して作成したチケットを右クリック（または…を押下）して削除する  
以下のリストア手順を実施  
画面を更新してリストアされていることを確認する  

## バックアップ手順
```
# データのバックアップ
$ docker exec -it redmine-db-container mysqldump -uroot -pPassword@9 redmine | sed '1d' > ./backup/data.sql
# ファイルのバックアップ
$ docker cp redmine-container:/usr/src/redmine/files/. ./backup/files/
```
補足）以下のWarningが出力される為、sedでその行を削除しています  
```
mysqldump: [Warning] Using a password on the command line interface can be insecure.
```

## リストア手順
```
# データのリストア
$ docker exec -i redmine-db-container mysql -uroot -pPassword@9 redmine < ./backup/data.sql
# ファイルのリストア
$ docker cp ./backup/files/. redmine-container:/usr/src/redmine/files/
```

## 課題
バックアップはスクリプト+cronで自動化しても良いかもしれない  

# nhk-gogaku-xml
NHK gogaku streaming xml files

NHK 語学講座のストリーミング配信の listdataflv.xml ファイルです。毎週実行してログを残すようにしているので，過去に配信されたストリーミングのファイル名が分かるはずです。

## このリポジトリの自動更新
listdataflv.xml の取得とリポジトリへのプッシュについては，もともとは手動で行っていたのですが，忘れることもあるかと思い自動化しました。
外部のサーバに以下のシェルスクリプトを置き，cron で定期的に実行を行っています。
README.md は GitHub で編集するので，最初に pull をしてから push しています。
```
% cat nhk-gogaku-xml-gitupdate.sh
#!/bin/sh

cd /path_to/nhk-gogaku-xml || exit;

git pull origin master

test -f xml-wget.sh && sh xml-wget.sh || echo "File doesn't exists"

git add -A 
git commit -am "`date '+%Y-%m-%d'` Updated files"
git push -u origin master
```
NHK のサーバでは毎週月曜日10:00に更新が行われるので，その後に更新用スクリプトの起動を行います。ここでは1時間後の11:00に起動するようにcrontabに書いています。
```
% crontab -l
0 11 * * 1 sh /path_to/nhk-gogaku-xml-gitupdate.sh
```

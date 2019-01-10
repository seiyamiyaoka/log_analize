## ダミーログを生成しデータベースに格納

### fluent dを使ってログを綺麗にする

https://github.com/uken/fluent-plugin-postgres

gemでfluentdを入れる

linuxのtdagentが元になっている

ダミーログを作成するgemをインストール

###  入力の設定

入力にログの情報を指定する。tailで下から読み取ります
formatを今回はapacheのログなのでapacheを指定
pathを実行コマンドに合わせるため`/tmp/dummy_access_log`を指定
一応タグを設定

### 出力の設定

postgresを使っているのでフォーマットを合わせてくれるgemを入れておく

あとはtypeを合わせて, port, username等を合わせる

あらかじめdbを作成しテーブルも作っておく



- 設定確認

```
fluentd -c test.conf -l debug.log &
```

- ログを指定箇所に吐き出している

```
apache-loggen --rate=10 --limit=10 --progress /tmp/dummy_access_log
```

- プロセスが増えてpsquelのguiが開かなくなったら
```
kill `ps -ef | grep fluent | awk '{print $2}'`
```

grepの出力からawkで取り出しkillする


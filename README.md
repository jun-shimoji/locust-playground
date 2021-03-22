- [構成](#構成)
- [docker使わずに並列実行する方法](#docker使わずに並列実行する方法)
    - [オプション設定を知る](#オプション設定を知る)
    - [masterをオプション設定した上で実行](#masterをオプション設定した上で実行)
    - [workerを実行](#workerを実行)
- [docker使用時の起動方法](#docker使用時の起動方法)
- [コンテナの消し方](#コンテナの消し方)
- [参考](#参考)

### 構成

```
.
├── README.md
├── docker-compose.yaml //dockerコンテナを複数立てたい時などに使う
└── locustfile.py //負荷条件を指定
```

* メモ
    * docker-composeを使うのは並列作業用だが、そもそもdocker使わなくてもよさそう


### docker使わずに並列実行する方法

#### オプション設定を知る
* --host https://example.com #送信先 -H だとだめみたい
* --headless #ヘッドレスモード
* --csv locust.master #結果出力
* --logfile locust.master.log #ログファイル
* -u #specifies the number of Users to spawn
* -r #specifies the spawn rate (number of users to start per second). # --step-time は使えないっぽい

#### masterをオプション設定した上で実行

```
locust -f ./locustfile.py \
            --run-time 5s \
            --headless \
            --master \
            --host $host \
            --csv locust.master \
            --expect-workers=2 \
            -u 1 \
            -r 1 \
            --logfile locust.master.log
```

#### workerを実行

以下を `--expect-workers` の数分実行する

```
locust -f ./locustfile.py --worker --master-host=127.0.0.1 --headless
```

----

### docker使用時の起動方法

(docker起動状態で)

```
docker-compose up -d
```

ブラウザから http://localhost:8089/ へアクセス

Hostを指定(http://master:8089がplaceholdされている)してGetリクエスト送信

### コンテナの消し方

```
docker rm $(docker ps -qa) --force
```

### 参考

* Running Locust distributed
    * https://docs.locust.io/en/stable/running-locust-distributed.html#running-locust-distributed
* Locustを1台のマシンで分散実行する（CLI編）
    * https://qiita.com/progrhyme/items/2bf7e9ad24baf2c24951
* Docker Compose
    * https://docs.locust.io/en/stable/running-locust-docker.html
* 初学者がlocust 1.0で負荷試験を行う
    * https://qiita.com/sekikatsu/items/992e82671aa505c5a652
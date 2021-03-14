### composition

```
.
├── README.md
├── docker-compose.yaml //dockerコンテナを複数立てたい時などに使う
└── locustfile.py //負荷条件を指定
```

### 起動方法

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
* Docker Compose
    * https://docs.locust.io/en/stable/running-locust-docker.html
* 初学者がlocust 1.0で負荷試験を行う
    * https://qiita.com/sekikatsu/items/992e82671aa505c5a652
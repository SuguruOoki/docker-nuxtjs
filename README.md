## docker-nuxtjs

### これは何

* Nuxt.jsアプリの開発環境です。Docker上にyarnや@vue/cli等がインストールされます
* Docker上でvue init nuxt-community/starter-templateがすぐに使えます

* ※Docker for WindowsやDocker Toolboxの場合、仮想マシン（docker-machine）を管理者権限で起動する必要があります

### 利用方法

#### Nuxtプロジェクト作成

``` bash
# --- hostで実行 ---
[After git clone]
$ cd ./docker-for-nuxtjs
$ mkdir your-nuxt-project
$ docker-compose up -d
$ docker-compose exec dev bash
# --- Docker上で実行 ---
$ cd your-nuxt-project
$ vue init nuxt-community/starter-template .
$ yarn
```

#### Dockerゲストのアプリに接続できるようにする

* package.jsonの修正(25行目付近に追加)

```json
  "config": {
    "nuxt": {
      "host": "0.0.0.0",
      "port": "3000"
    }
  }
```

#### ホットリロード対応

* nuxt.config.tsの修正(37行目付近に追加)

```ts
  watchers: {
    webpack: {
      poll: true
    }
  }
```

#### サーバー起動

```bash
$ yarn run dev
# Terminalに、「OPEN  http://localhost:3000」が出力されるまで待つ必要があります
```

#### Dockerコンテナを停止する

```bash
# --- hostで実行 ---
$ docker-compose down
```

#### 開発を再開する

```bash
# --- hostで実行 ---
$ docker-compose up -d
$ docker-compose exec dev bash
# --- Docker上で実行 ---
$ cd project
$ yarn run dev
```

### よくある問題

#### Step13/25: RUN bash remi.shでエラーになる

* dev/remi.shやdev/direnv.shの改行コードがCRLFになっている可能性があるので、LFを指定


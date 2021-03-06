# Basic 認証も可能な AWS S3 へのリバースプロキシ


## 概要

指定した S3 バケット にリバースプロキシする Web サービスです。  
API 経由でアクセスするため、バケットに静的 Web サイトホスティングの設定は不要です。  
オプションでフロントに Basic 認証がかけられます。

http://this-proxy.com/access/ -> s3://backet/access/index.html


## 使い方

### 1. 環境変数をセットします

環境変数                   | 説明                                             | 必須    | 初期値
------------------------- | ----------------------------------------------- | ------ | ---
AWS_S3_BUCKET             | プロキシ先の S3 バケット                           | *       | 
AWS_S3_KEY_PREFIX         | S3 オブジェクトにプリフィクス文字列があるなら指定       |        | -
AWS_REGION                | バケットの存在する AWS リージョン                    |        | us-east-1
AWS_ACCESS_KEY_ID         | API を使うための AWS アクセスキー                   |        | EC2 インスタンスロール
AWS_SECRET_ACCESS_KEY     | API を使うための AWS シークレットキー                |        | EC2 インスタンスロール
HTTP_CACHE_CONTROL        | S3 の `Cache-Control` 属性を上書きして返します      |        | S3 オブジェクト属性値
HTTP_EXPIRES              | S3 の `Expires` 属性を上書きして返します            |        | S3 オブジェクト属性値
BASIC_AUTH_USER           | Basic 認証をかけるなら、その `ユーザ名`              |        | -
BASIC_AUTH_PASS           | Basic 認証をかけるなら、その `パスワード`            |        | -
SSL_CERT_PATH             | TLS を有効にしたいなら、その `cert.pem` へのパス     |        | -
SSL_KEY_PATH              | TLS を有効にしたいなら、その `key.pem` へのパス      |        | -
APP_PORT                  | このサービスが待機する `ポート番号`                  |        | 80
ACCESS_LOG                | 標準出力へアクセスログを送る                        |        | false

### 2. アプリを起動します

`docker run -d -p 8080:80 -e AWS_REGION -e AWS_S3_BUCKET pottava/s3-proxy`

* Basic 認証をつけるなら:  

`docker run -d -p 8080:80 -e AWS_REGION -e AWS_S3_BUCKET -e BASIC_AUTH_USER -e BASIC_AUTH_PASS pottava/s3-proxy`

* TLS を有効にしたいなら:  

`docker run -d -p 8080:80 -e AWS_REGION -e AWS_S3_BUCKET -e SSL_CERT_PATH -e SSL_KEY_PATH pottava/s3-proxy`

* docker-compose.yml として使うなら:  

```
proxy:
  image: pottava/s3-proxy
  ports:
    - 8080:80
  environment:
    - AWS_REGION=ap-northeast-1
    - AWS_ACCESS_KEY_ID
    - AWS_SECRET_ACCESS_KEY
    - AWS_S3_BUCKET
    - BASIC_AUTH_USER=admin
    - BASIC_AUTH_PASS=password
    - ACCESS_LOG=true
  container_name: proxy
```

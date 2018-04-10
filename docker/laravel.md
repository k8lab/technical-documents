# laravel開発環境

## 構成

当ドキュメントの環境構成のOSは「Alpine Linux」ベースを使用します。

- nginx:1.13.11-alpine
- php:7.2.4-fpm-alpine3.7
- postgres:9.6.8-alpine

またホスト端末はMac

## php-fpm関連

### extentionインストール

extentionを追加する場合は、下記のコマンドを実行する。  
extentionに必要なライブラリがあれば実行前にインストールしておく。

```bash
docker-php-ext-install pdo_pgsql
```

### peclインストール

Alpineを使用している場合、コンパイルツールがインストールされていない。  
※phpインストール時に一時的にインストールされ削除されている。
その為、以下のように一時的にコンパイルツールをインストールする必要がある。

```bash
apk add --no-cache --virtual .build-deps $PHPIZE_DEPS && \
pecl install xdebug && \
docker-php-ext-enable xdebug && \
apk del .build-deps
```

### Macでのリモートデバッグ

- デフォルトではホスト端末もしくはリモート端末のアドレスのポートの9000番へのアクセスが可能である必要がある。
- コンテナ内(php-fpm)へリクエストを行った際のリモートアドレスはデフォルトネットワークの設定では「172.18.0.1」となる。
- 仮想環境であればxdebug.remote_hostへリモートアドレスを指定すれば開発環境(NetBeans、VSCode等)へ接続される。
- ただし、「Docker for Mac」においては接続が行われない。(コンテナ上からリモートアドレスへの疎通が出来ない)
- 特殊な設定が必要でありxdebug.remote_hostへは「docker.for.mac.localhost」を指定する必要がある。ただし、17.12.0からは「docker.for.mac.host.internal」が追加、18.0.3からは「host.docker.internal」追加されこちらが推奨される。
- 詳細は次のドキュメントを参照　[Networking features in Docker for Mac](https://docs.docker.com/docker-for-mac/networking/)

### Alpine Linux パッケージ操作参考

https://qiita.com/pottava/items/970d7b5cda565b995fe7
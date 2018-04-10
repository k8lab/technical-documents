# laravel開発環境

## 構成

当ドキュメントの環境構成のOSは「Alpine Linux」ベースを使用します。

## php-fpm関連

### extentionインストール

extentionを追加する場合は、下記のコマンドを実行する。  extentionに必要なライブラリがあれば実行前にインストールしておく。

```bash
docker-php-ext-install pdo_pgsql
```

### peclインストール

alpineを使用している場合、コンパイルツールがインストールされていない。  
その為、以下のように一時的にコンパイルツールをインストールする必要がある。

```bash
apk add --no-cache --virtual .build-deps $PHPIZE_DEPS && \
pecl install xdebug && \
docker-php-ext-enable xdebug && \
apk del .build-deps
```

## Alpine

https://qiita.com/pottava/items/970d7b5cda565b995fe7
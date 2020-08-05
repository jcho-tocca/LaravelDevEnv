Laravel 開発環境
====

# 概要
Dockerを使ってローカル開発環境を構築する

# 開発環境

VirtualBox 6.1  
Docker Client 19.03.1  
Docker Engine 19.03.5  
Composer version 1.10.6  
PHP:7.3.3-apache  
MySQL 5.6   
phpMyAdmin 4.7  
node:13.7  
MailHog  

# 構築手順
## 1. Dockerfile、docker-compose.ymlファイルダウンロード
```bash
git svn clone https://github.com/jcho-tocca/LaravelDevEnv/trunk/Docker project_name
```
## 2. httpd.conf 修正
apache/000-default.conf  
環境に合わせて内容を修正してください。
## 3. php.ini 修正
php/php.ini  
環境に合わせて内容を修正してください。
## 4. コマンド実行
```bash
cd project_name
docker-compose up -d --build
cd www
rm html
composer create-project laravel/laravel --prefer-dist .
mv public/ html/
```

## 5. composer、npm、artisan を実行したい場合
### 例
```bash
docker-compose run --rm composer update
docker-compose run --rm npm run dev
docker-compose run --rm artisan migrate 
```

# 確認

## Apache
http://[Docker Machine IP]/index.php  
https://[Docker Machine IP]/index.php

## phpMyAdmin
http://[Docker Machine IP]:8080

## DB情報
HostName : Docker Machine IP  
Port : 33060  
Root Name : root  
Root Password : root  
User Name : dev  
User Password : dev  
DB : development  

## MailHog
http://[Docker Machine IP]:8025

# SSH接続
```bash
docker-compose exec コンテナ名 bash
```

# 設定ファイルパス
## Apache
/etc/apache2/sites-available/000-default.conf
## PHP
/usr/local/etc/php/php.ini

# httpd.conf、php.iniを修正して反映する
```bash
docker-compose up -d --build
```
# PHP のバージョン切替
Dockerfileの修正  
```bash
php:<version>-apache
```
公式ページ：https://hub.docker.com/_/php
# PHPの拡張モジュールを追加
Dockerfileを修正して対応

# トラブルシューティング

下記のエラー発生時(apt-get update時)、ローカルのDNSサーバーのアドレスを  
Public Domain(1.1.1.1 or 8.8.8.8など)にすると解決する場合がある

```bash
Err:1 http://security.debian.org/debian-security .......
```
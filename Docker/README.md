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
## 2. httpd.conf、ssl.conf 修正
apache/000-default.conf  
default-ssl.conf  
環境に合わせて内容を修正してください。
## 3. php.ini 修正
php/php.ini  
環境に合わせて内容を修正してください。
## 4. コマンド実行
```bash
cd project_name
docker-compose up -d --build
docker-compose run --rm composer create-project --prefer-dist laravel/laravel .
```

## 5. composer、npm、artisan を実行したい場合
### 例
```bash
docker-compose run --rm composer update
docker-compose run --rm npm run dev
docker-compose run --rm artisan migrate 
```
### 注意
windows使用者は docker-compose run --rm npm はエラーが発生しますのでご使用にならずにクライアント環境	にnpmをインストールしてご使用ください。

windows環境ではDocker volume mount の問題があります。 

・volume mount によって空になる問題  
・Linuxのシンボールリンクの作成ができない問題  

参考ページ：https://castaneai.hatenablog.com/entry/2019/01/29/151257  

重くはなりますがwindows 環境で仮想環境を用意すれば回避できると思います。  
[こちらへ](https://github.com/jcho-tocca/LaravelDevEnv/tree/master/DockerOnVagrant)

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
# XDebug 設定
php.ini  
```bash
[XDebug]
xdebug.remote_enable = 1
xdebug.remote_autostart = 1
xdebug.remote_connect_back = 0
xdebug.remote_host = [VirtualBox Host-Only Ethernet Adapter IP]
xdebug.remote_port = 9000
xdebug.var_display_max_children = -1
xdebug.var_display_max_data = -1
xdebug.var_display_max_depth = -1
```
vscode - launch.json  
```json
"name": "Listen for XDebug",
"type": "php",
"request": "launch",
"port": 9000,
"pathMappings": {
	// コンテナ上のDocument Root : ローカルのDocument Root
	"/var/www/html/public":"C:\\LaravelDevEnv\\Docker\\html\\public" // ご自分の環境に合わせてください。
}
```
# トラブルシューティング

下記のエラー発生時(apt-get update時)、ローカルのDNSサーバーのアドレスを  
Public Domain(1.1.1.1 or 8.8.8.8など)にすると解決する場合がある

```bash
Err:1 http://security.debian.org/debian-security .......
```
Laravel 開発環境
====

## 概要
Dockerを使ってローカル開発環境を構築する

## 開発環境

VirtualBox 6.1  
Docker Client 19.03.1  
Docker Engine 19.03.5  
Composer version 1.10.6
PHP:7.3.3-apache  
MySQL 5.6   
phpMyAdmin 4.7  

# 構築手順

```bash
git svn clone https://github.com/jcho-tocca/LaravelDevEnv/trunk/Docker project_name
cd project_name
docker-compose up -d
cd www
composer create-project laravel/laravel --prefer-dist .
mv public/ html/
```

# 確認

## Apache
http://[Docker Machine IP]/index.php

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
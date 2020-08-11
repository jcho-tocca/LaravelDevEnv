Laravel 開発環境
====

## 概要
composerを使ってローカル開発環境を構築する

## 1. グローバルインストール
```bash
composer global require laravel/installer
laravel new project_name
cd project_name
php artisan serve
```

## 2. プロジェクトごとにインストール
```bash
composer create-project --prefer-dist laravel/laravel project_name
cd project_name
php artisan serve
```

## アクセス
http://localhost::8000
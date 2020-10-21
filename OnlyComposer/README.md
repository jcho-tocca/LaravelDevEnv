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
### 最新バージョン
```bash
composer create-project --prefer-dist laravel/laravel project_name
```
### バージョン指定

```bash
composer create-project --prefer-dist laravel/laravel:バージョン project_name
```
バージョン情報  
https://packagist.org/packages/laravel/laravel
## 3. サーバ起動
```bash
cd project_name
php artisan serve
```

## 4. アクセス
http://localhost:8000
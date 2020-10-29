Laravel 開発環境
====

## 概要
Homesteadを使ってローカル開発環境を構築する
## 必須事項
Windows10  
Composer  
Vagrant  
VirtualBox 6.x  
## 構築手順
### 1. コマンド実行
```bash
mkdir プロジェクトフォルダ
cd プロジェクトフォルダ
composer require laravel/homestead --dev
vendor\\bin\\homestead make
cd ~/.ssh
ssh-keygen -t rsa
エンター
パスワード入力
```
### 2. Homestead.yml
```yaml
ip: 指定IP
memory: 2048
cpus: 2
provider: virtualbox
ssl: true
authorize: ~/.ssh/id_rsa.pub
keys:
    - ~/.ssh/id_rsa
folders:
    -
        map: 'C:\プロジェクトフォルダ\code'
        to: /home/vagrant/code
sites:
    -
        map: ドメイン.local
        to: /home/vagrant/code/public
        type: "apache"
        php: "バージョン"
        options:
            owner: "root"
            group: "root"
            mount_options: ['dmode=777','fmode=777']
databases:
    - データベース名
features:
    -
        mariadb: false
    -
        ohmyzsh: false
    -
        webdriver: false
name: vm名
hostname: ホストネーム
```
### 3. after.sh 追記( phpMyAdmin インスール )
```
if [ -d /home/vagrant/code/public/phpmyadmin ]; then
    sudo echo '--- phpMyAdmin already installed. --- '
else
    sudo wget -k https://files.phpmyadmin.net/phpMyAdmin/5.0.2/phpMyAdmin-5.0.2-all-languages.tar.gz
    sudo tar -xzf phpMyAdmin-5.0.2-all-languages.tar.gz -C /home/vagrant/
    sudo rm phpMyAdmin-5.0.2-all-languages.tar.gz
    sudo mv /home/vagrant/phpMyAdmin-5.0.2-all-languages/ /home/vagrant/code/public/phpmyadmin/
    sudo echo '--- phpMyAdmin 4.8.1 install complete ---'
fi
```
### 4. hosts ファイル修正
```bash
指定IP  ドメイン.local
```
### 5. vagrant 実行
```bash
vagrant up
```
### 6. Laravel インストール
```bash
vagrant ssh
cd ~/code
composer create-project laravel/laravel --prefer-dist .
```
### 7. アクセス
http://ドメイン.local  
http://ドメイン.local/phpmyadmin

### 8. アクセスできない場合
方法1
```bash
vagrant provision
```

方法2
```bash
vagrant ssh
sudo apachectl restart
#もしくは
sudo service apache2 start
```
### 9. SSL対応(apache使用時)
```
vagrant ssh
cp /etc/ssl/certs/ca.homestead.[ホストネーム].crt /home/vagrant/code
```
エクスプローラを開きca.homestead.[ホストネーム].crtをクリックすると、証明書が開きます。  
![1](https://user-images.githubusercontent.com/64624465/97399694-069d5c00-1931-11eb-92b2-48dbdf3f9c61.png)

[証明書をインストール]をクリックします。  
![2](https://user-images.githubusercontent.com/64624465/97399698-0735f280-1931-11eb-9633-3ade2ff5d1fb.png)

[証明書のインポートウィザード]が開きます。[現在のユーザ]を選択して[次へ]  
![3](https://user-images.githubusercontent.com/64624465/97399700-07ce8900-1931-11eb-8ea5-824fbed06593.png)

[証明書ストア]のページは、[証明書をすべて次のストアに配置する]を選択肢、[参照]をクリックして[証明書ストアの選択]を開きます。  
![4](https://user-images.githubusercontent.com/64624465/97399703-07ce8900-1931-11eb-875c-18e0e3728604.png)

[信頼されたルート証明機関]を選択して[OK]をクリックします。ダイアログが閉じたら[次へ]をクリック  
![5](https://user-images.githubusercontent.com/64624465/97399704-08671f80-1931-11eb-9ab3-81d3053349d2.png)

[完了]をクリック  
![6](https://user-images.githubusercontent.com/64624465/97399706-08671f80-1931-11eb-97f5-89553bc51059.png)

[セキュリティの警告]が表示されるので、[はい]をクリック  
![7](https://user-images.githubusercontent.com/64624465/97399708-08ffb600-1931-11eb-9b05-55721be09175.png)

完了したら、証明書は削除しておきます  
```bash
rm ~/code/ca.homestead.[ホストネーム].crt
```
### 11. Visual Studio Code + Xdebug環境構築
```bash
vagrant ssh
sudo vi /etc/php/7.4/fpm/conf.d/20-xdebug.ini
```
下記を追記
```bash
zend_extension=xdebug.so
xdebug.remote_enable = 1
xdebug.remote_autostart = 1
xdebug.remote_connect_back = 1
xdebug.remote_port = 9000
xdebug.max_nesting_level = -1
```
サーバ再起動
```bash
vagrant reload
```
vscodeのlaunch.json修正
```json
{
	"version": "0.2.0",
	"configurations": [
		{
			"name": "Listen for XDebug",
			"type": "php",
			"request": "launch",
			"port": 9000,
			"pathMappings": {
				"/home/vagrant/code": "C:\\プロジェクトフォルダ\\code"
            }
		},
		{
			"name": "Launch currently open script",
			"type": "php",
			"request": "launch",
			"program": "${file}",
			"cwd": "${fileDirname}",
			"port": 9000
		}
	]
}
```
### 11. Homestead.yml に修正をした場合
```bash
vagrant reload --provision
```
## DB 接続情報
### MySQL
ホスト：127.0.0.1  
ポート：33060  
もしくは  
ホスト：指定IP  
ポート：3306  
ユーザー名：homestead  
パスワード：secret

### PostgreSQL
ホスト：127.0.0.1  
ポート：54320  
もしくは  
ホスト：指定IP  
ポート：5432  
ユーザー名：homestead  
パスワード：secret

## Mailhog
SMTP情報  
ホスト：localhost  
ポート：1025  
.env設定
```bash
MAIL_MAILER=smtp
MAIL_HOST=localhost
MAIL_PORT=1025
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
```
ブラウザで確認  
http://127.0.0.1:8025  
http://localhost:8025  
http://指定IP:8025  
http://ドメイン:8025  

## 公式ドキュメント
### 英語
https://laravel.com/docs/7.x/homestead
### 日本語
https://readouble.com/laravel/7.x/ja/homestead.html
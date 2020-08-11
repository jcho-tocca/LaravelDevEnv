Laravel 開発環境
====

# 概要
Vagrant + Ubuntu + Dockerを使ってローカル開発環境を構築する  
多くの現場ではLinux環境を使用しています。

# 開発環境

VirtualBox 6.1  
Vagrant 2.2.9  
Ubuntu-16.04  
Docker Client latest  
Docker Engine latest  
Composer version 1.10.6  
PHP:7.3.3-apache  
MySQL 5.6   
phpMyAdmin 4.7  
node:13.7  
MailHog  

# 構築手順
## 1. Vagrantfile ダウンロード
```bash
git svn clone https://github.com/jcho-tocca/LaravelDevEnv/trunk/DockerOnVagrant project_name
cd project_name
```
## 2. 仮想環境実行
```bash
vagrant up
```
## 3. パッケージを更新
```bash
vagrant ssh
sudo apt-get update
sudo apt-get upgrade
```
「Configuring grub-pc」の画面が出たら、TabでOKをフォーカスしてEnter

## 4. ubuntu-desktop インストール
```bash
sudo apt-get install ubuntu-desktop
```
すべてyesで進めていきます。
かなり時間がかかります。

## 5. 再起動
```bash
sudo reboot
```
## 6. Docker Engine インストール
公式サイト : https://docs.docker.com/engine/install/ubuntu/  

```bash
# Uninstall old versions
sudo apt-get remove docker docker-engine docker.io containerd runc

# SET UP THE REPOSITORY
sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

# Add Docker’s official GPG key:
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Verify that you now have the key with the fingerprint 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88, 
# by searching for the last 8 characters of the fingerprint.
sudo apt-key fingerprint 0EBFCD88

# Use the following command to set up the stable repository. 
# To add the nightly or test repository, add the word nightly or test (or both) after the word stable in the commands below.
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

# INSTALL DOCKER ENGINE
# バージョン指定のインストールも可能です。公式サイトをご覧ください。
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io

# docker-composeをインストールする
sudo curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose

## バス追加
sudo chmod +x /usr/local/bin/docker-compose
sudo vi ~/.bashrc 
------------------------
export PATH=$PATH:/usr/local/bin/
------------------------
# 確認
sudo docker run hello-world
```
## sudo 入力しない方法
### 方法1
```bash
sudo su
```
### 方法2
```bash
sudo usermod -aG docker ${USER}
sudo service docker restart
```

## 7. Gitをインストールする
```bash
sudo apt-get install git
```

## 8. Laravel 開発環境
下記の手順で進めてください。  

[Laravel 開発環境構築](https://github.com/jcho-tocca/LaravelDevEnv/tree/master/Docker#%E6%A7%8B%E7%AF%89%E6%89%8B%E9%A0%86)


## 9. ubuntu-desktopにログイン
id : vagrant  
pw : vagrant  

上記のアカウントでログインしてご使用ください。

# トラブルシューティング

## ■ virtualbox - メモリがreadになることはできませんでした  
設定 -> ディスプレイ -> スクリーンタブ -> グラフィックコントローラー -> VBoxSVGA に変更して再起動  
## ■ composer - proc_open(): fork failed - Out of memory
「スワップファイル」を作って回避する  
```bash
sudo -s /bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024
sudo -s /sbin/mkswap /var/swap.1
sudo -s /sbin/swapon /var/swap.1
```
## ■ アクセス時 - The stream or file "/var/www/html/storage/logs/laravel.log"
```bash
docker-compose exec web bash
chown www-data storage/ -R
```

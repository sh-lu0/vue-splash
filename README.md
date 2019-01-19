# vue-splash

# Laravel プロジェクトを作成する
## Laravel Valet
ローカルに環境を構築するため、PC 自体に PHP やデータベースがインストールされている必要がある．
### Homebrew
```$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"```

### PHP
```$ brew install php@7.2```

### Composer
```
$ php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
$ php -r "if (hash_file('SHA384', 'composer-setup.php') === '93b54496392c062774670ac18b134c3b3a95e5a5e5c8f1a9f115f203b75bf9a129d5daa8ba6a13e2cc8a1da0806388a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
$ php composer-setup.php
$ php -r "unlink('composer-setup.php');"
$ mv composer.phar /usr/local/bin/composer
```

### PATHを通す
```
export PATH=$PATH:~/.composer/vender/bin
```
をbash_profileに追加．
```$ source ~/.bash_profile```
で更新

### Valet のインストールと起動
```
$ composer global require laravel/valet
$ valet install
```

### アプリケーションの配信
プロジェクトの中に入って
```
$ mkdir ~/Projects/laravel-app
$ cd ~/Projects/laravel-app
$ valet park
$ composer create-project laravel/laravel hoge
```
```http://hoge.test``` という URL でアプリケーションが配信される．

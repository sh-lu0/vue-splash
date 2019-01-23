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

## プロジェクト作成
プロジェクトの中に入って
```
$ mkdir ~/Projects/laravel-app
$ cd ~/Projects/laravel-app
$ valet park
$ composer create-project laravel/laravel hoge
```
```http://hoge.test``` という URL でアプリケーションが配信される．

## データベース作成
Homebrewを使用してPostgreSQLをインストール
```brew install postgresql```
データベース作成
```
$ psql -U postgres
Password for user postgres: 
psql (9.6.3)
Type "help" for help.

postgres=# CREATE DATABASE vuesplash;
CREATE DATABASE
postgres=# \q
```

## Laravelの設定
### app.php
```config/app.php``` の ```locale```設定を日本語にする．
```
'locale' => 'ja',
```

### .env
```
APP_NAME=Vuesplash

DB_CONNECTION=pgsql
DB_HOST=127.0.0.1
DB_PORT=5432
DB_DATABASE=vuesplash
DB_USERNAME=postgres
DB_PASSWORD=postgres
```

### EditorConfig
```
[*.{yml,json,scss,html,js,vue,blade.php}]
indent_size = 2
```

## 確認
```$ valet start```

### エラー①
```http://vuesplash.test``` にアクセス
エラーが出た．
```
Homebrew PHP appears not to be linked.
```
解決法：homebrewでPHPへのリンクをきちんと貼る．
```
$ brew link php71 --force
```

### エラー②
```http://vuesplash.test``` にアクセス
502 Bad Gateway error

解決法：
```
$ sudo brew services restart nginx && sudo brew services restart php71
$ brew uninstall php71
$ brew install php@7.2
$ valet install
$ brew link php72 --force
```
無事アクセスできた

---

## migrateできない（未解決）
```$ php artisan migrate```を実行するとエラー

```
Illuminate\Database\QueryException  : SQLSTATE[08006] [7] FATAL:  password authentication failed for user "postgres" (SQL: select * from information_schema.tables where table_schema = public and table_name = migrations)

  at /Users/chiaki/Projects/vue-splash/vuesplash/vendor/laravel/framework/src/Illuminate/Database/Connection.php:664
    660|         // If an exception occurs when attempting to run a query, we'll format the error
    661|         // message to include the bindings with SQL, which will make this exception a
    662|         // lot more helpful to the developer instead of just the database's errors.
    663|         catch (Exception $e) {
  > 664|             throw new QueryException(
    665|                 $query, $this->prepareBindings($bindings), $e
    666|             );
    667|         }
    668|

  Exception trace:

  1   PDOException::("SQLSTATE[08006] [7] FATAL:  password authentication failed for user "postgres"")
      /Users/chiaki/Projects/vue-splash/vuesplash/vendor/laravel/framework/src/Illuminate/Database/Connectors/Connector.php:70

  2   PDO::__construct("pgsql:host=127.0.0.1;dbname=vuesplash;port=5432;sslmode=prefer", "postgres", "postgres", [])
      /Users/chiaki/Projects/vue-splash/vuesplash/vendor/laravel/framework/src/Illuminate/Database/Connectors/Connector.php:70

  Please use the argument -v to see more details.
  ```

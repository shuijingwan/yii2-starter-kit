# 安装

## 目录
- [在你开始之前](#在你开始之前)
- [手动安装](#手动安装)
    - [要求](#要求)
    - [安装应用程序](#安装应用程序)
    - [配置Web服务器](#配置Web服务器)

- [Docker安装](#Docker安装)
- [Vagrant安装](#Vagrant安装)
- [单域名安装](#单域名安装)
- [演示用户](#演示用户)
- [重要提示](#重要提示)

## 在你开始之前
1. 如果您还没有安装 [Composer](http://getcomposer.org/)，您可以按照 [getcomposer.org](http://getcomposer.org/doc/00-intro.md#installation-nix) 中的说明进行安装。

2. 安装用于Yii前端资源管理所需的 Composer asset 插件
```bash
composer global require "fxp/composer-asset-plugin"
```

3. 安装 NPM 或 Yarn 来构建前端脚本
- [NPM] (https://docs.npmjs.com/getting-started/installing-node)
- Yarn (https://yarnpkg.com/en/docs/install)

### 获取源代码
#### 下载地址
https://github.com/trntv/yii2-starter-kit/archive/master.zip

#### 或手动克隆Git仓库
```
git clone https://github.com/trntv/yii2-starter-kit.git
```
#### 安装 Composer 依赖项
```
composer install
```

### 通过 Composer 获取源代码
您可以使用 `composer` 的以下命令来安装此应用程序模板：

```
composer create-project --prefer-dist --stability=dev trntv/yii2-starter-kit
```

## 手动安装

### 要求
此应用程序模板的最低要求是您的Web服务器支持PHP 5.5.0。
必需的PHP扩展：
- intl
- gd
- mcrypt
- com_dotnet (for Windows)



### 安装应用程序
1. 在项目根目录中，将 `.env.dist` 复制为 `.env` 。
2. 调整 `.env` 文件中的配置
	- 设置调试模式和您当前的环境
	```
	YII_DEBUG   = true
	YII_ENV     = dev
	```
	- 设置数据库配置
	```
	DB_DSN           = mysql:host=127.0.0.1;port=3306;dbname=yii2-starter-kit
	DB_USERNAME      = user
	DB_PASSWORD      = password
	```

	- 设置应用程序的应用网址
	```
	FRONTEND_HOST_INFO    = http://yii2-starter-kit.dev
	BACKEND_HOST_INFO     = http://backend.yii2-starter-kit.dev
	STORAGE_HOST_INFO     = http://storage.yii2-starter-kit.dev
	```

3. 在命令行中运行
```
php console/yii app/setup
npm install
npm run build
```

### 配置Web服务器
- 将 `docker/vhost.conf` 复制到您的 nginx 配置目录
- 更改以适应您的环境

## Docker 安装
1. 遵循 [docker 安装](https://docs.docker.com/engine/installation/) 指令
2. 将 ``127.0.0.1 yii2-starter-kit.dev backend.yii2-starter-kit.dev storage.yii2-starter-kit.dev``* 添加到你的 `hosts` 文件
2. 在项目根目录中，将 `.env.dist` 复制为 `.env` 
3. 运行 `docker-compose build`
4. 运行 `docker-compose up -d`
5. 通过 `docker-compose exec app bash` 登录应用程序容器
6. 安装 composer 的指令可参考 [Composer](https://getcomposer.org/download/)
7. 运行 `php composer.phar global require "fxp/composer-asset-plugin"` and `php composer.phar install --profile --prefer-dist -o -v`
- 如果要求您从 [github account] (https://github.com/settings/tokens) 获取令牌.
8. 使用 `php ./console/yii app/setup --interactive=0` 设置应用程序
9. 使用 `exit` 退出应用程序容器
10. 安装就绪 - 打开 http://yii2-starter-kit.dev 进行访问

 * - docker 主机IP地址在 Windows 和 MacOS 系统上可能有所不同
 
*PS* 也可以在应用程序容器中使用 bash. 需要运行 `docker-compose exec app bash`

### Docker 常见问题
1. 如何在容器外部运行 yii 控制台命令?

`docker-compose exec app console/yii help`

`docker-compose exec app console/yii migrate`

`docker-compose exec app console/yii rbac-migrate`

2. 如何使 workbench, navicat 等连接到应用程序数据库?
MySQL 可用配置为 `yii2-starter-kit.dev`, port `3306`. User - `root`, password - `root`

## Vagrant 安装
如果需要，您可以使用打包好的 Vagrant 而不是安装应用程序到本地计算机.

1. 安装 [Vagrant](https://www.vagrantup.com/)
2. 将目录 `docs/vagrant-files` 下的文件复制到应用程序根目录
3. 复制 `./vagrant/vagrant.yml.dist` 为 `./vagrant/vagrant.yml`
4. 创建 GitHub [personal API token](https://github.com/blog/1509-personal-api-tokens)
5. 根据需要编辑 `./vagrant/vagrant.yml` 文件，包括添加 GitHub personal API token
6. 运行:
```
vagrant plugin install vagrant-hostmanager
vagrant up
```
安装就绪. 打开 http://yii2-starter-kit.dev 进行访问

## 演示数据
### 演示用户
```
Login: webmaster
Password: webmaster

Login: manager
Password: manager

Login: user
Password: user
```

## 单域名安装
### 安装应用程序
调整 `.env` 文件中的配置

```
FRONTEND_BASE_URL    = /
BACKEND_BASE_URL     = /admin
STORAGE_BASE_URL     = /storage/web
```

调整 `backend/config/web.php` 文件中的配置
```
    ...
    'components'=>[
        ...
        'request' => [
            'baseUrl' => '/admin',
        ...
```
调整 `frontend/config/web.php` 文件中的配置
```
    ...
    'components'=>[
        ...
        'request' => [
            'baseUrl' => '',
        ...
```

### 配置Web服务器
##### Apache下的单域名
这是一个单个域名下的Apache配置示例
```
<VirtualHost *:80>
    ServerName yii2-starter-kit.dev

    RewriteEngine on
    # the main rewrite rule for the frontend application
    RewriteCond %{HTTP_HOST} ^yii2-starter-kit.dev$ [NC]
    RewriteCond %{REQUEST_URI} !^/(backend/web|storage/web)
    RewriteRule !^/frontend/web /frontend/web%{REQUEST_URI} [L]
    # redirect to the page without a trailing slash (uncomment if necessary)
    #RewriteCond %{REQUEST_URI} ^/backend/$
    #RewriteRule ^(/backend)/ $1 [L,R=301]
    # disable the trailing slash redirect
    RewriteCond %{REQUEST_URI} ^/backend$
    RewriteRule ^/backend /backend/web/index.php [L]
    # the main rewrite rule for the backend application
    RewriteCond %{REQUEST_URI} ^/backend
    RewriteRule ^/backend(.*) /backend/web$1 [L]

    DocumentRoot /your/path/to/yii2-starter-kit
    <Directory />
        Options FollowSymLinks
        AllowOverride None
        AddDefaultCharset utf-8
    </Directory>
    <Directory "/your/path/to/yii2-starter-kit/frontend/web">
        RewriteEngine on
        # if a directory or a file exists, use the request directly
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteCond %{REQUEST_FILENAME} !-d
        # otherwise forward the request to index.php
        RewriteRule . index.php

        Require all granted
    </Directory>
    <Directory "/your/path/to/yii2-starter-kit/backend/web">
        RewriteEngine on
        # if a directory or a file exists, use the request directly
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteCond %{REQUEST_FILENAME} !-d
        # otherwise forward the request to index.php
        RewriteRule . index.php

        Require all granted
    </Directory>
    <Directory "/your/path/to/yii2-starter-kit/storage/web">
        RewriteEngine on
        # if a directory or a file exists, use the request directly
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteCond %{REQUEST_FILENAME} !-d
        # otherwise forward the request to index.php
        RewriteRule . index.php

        Require all granted
    </Directory>
    <FilesMatch \.(htaccess|htpasswd|svn|git)>
        Require all denied
    </FilesMatch>
</VirtualHost>
```

#### Nginx下的单域名
这是一个单个域名下的Nginx配置示例

```
server {
    listen 80;

    root /var/www;
    index index.php index.html;

    server_name yii2-starter-kit.dev;

    charset utf-8;

    # location ~* ^.+\.(jpg|jpeg|gif|png|ico|css|pdf|ppt|txt|bmp|rtf|js)$ {
    #   access_log off;
    #   expires max;
    # }

    location / {
        root /var/www/frontend/web;
        try_files $uri /frontend/web/index.php?$args;
    }

    location /backend {
        try_files  $uri /backend/index.php?$args;
    }

    # storage access
    location /storage {
        try_files  $uri /storage/web/index.php?$args;
    }

    client_max_body_size 32m;

    location ~ \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_pass php-fpm;
        fastcgi_index index.php;
        include fastcgi_params;

        ## Cache
        # fastcgi_pass_header Cookie; # fill cookie valiables, $cookie_phpsessid for exmaple
        # fastcgi_ignore_headers Cache-Control Expires Set-Cookie; # Use it with caution because it is cause SEO problems
        # fastcgi_cache_key "$request_method|$server_addr:$server_port$request_uri|$cookie_phpsessid"; # generating unique key
        # fastcgi_cache fastcgi_cache; # use fastcgi_cache keys_zone
        # fastcgi_cache_path /tmp/nginx/ levels=1:2 keys_zone=fastcgi_cache:16m max_size=256m inactive=1d;
        # fastcgi_temp_path  /tmp/nginx/temp 1 2; # temp files folder
        # fastcgi_cache_use_stale updating error timeout invalid_header http_500; # show cached page if error (even if it is outdated)
        # fastcgi_cache_valid 200 404 10s; # cache lifetime for 200 404;
        # or fastcgi_cache_valid any 10s; # use it if you want to cache any responses
    }
}

## PHP-FPM Servers ##
upstream php-fpm {
    server app:9000;
}
```

## 重要提示
- 有一个与sendfile相关的VirtualBox bug，可能导致文件损坏，如果你使用Vagrant，在你的nginx配置中设置：
```sendfile off;```

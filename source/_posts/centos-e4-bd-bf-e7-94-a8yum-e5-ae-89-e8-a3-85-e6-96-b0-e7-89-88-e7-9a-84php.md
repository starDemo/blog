---
title: Centos使用yum安装新版的PHP
url: 3546.html
id: 3546
categories:
  - Linux
  - 运维
date: 2017-10-30 15:26:57
tags:
---

RT 为什么想干运维 嗯 每天装服务器 怎么会有这么多服务器环境要装 嗯 yum解决 不想编译了 偷懒 新版PHP7.1 装法 CentOS/RHEL 7.x:

    rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
    rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm

如果是centos6，那么执行以下代码： CentOS/RHEL 6.x:

    rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
    rpm -Uvh https://mirror.webtatic.com/yum/el6/latest.rpm

嗯 安装好了后使用下边的命令 yum install php71w php71w-common php71w-fpm php71w-opcache php71w-gd php71w-mysqlnd php71w-mbstring php71w-pecl-redis php71w-pecl-memcached php71w-devel 然后 bingo   一般都要安装memcached，gd,mysql,等等是吧？

    安装包         提供的拓展
    php70w          mod_php , php70w-zts
    php70w-bcmath
    php70w-cli      php-cgi, php-pcntl, php-readline
    php70w-common   php-api, php-bz2, php-calendar, php-ctype, php-curl, php-date, php-exif, php-fileinfo, php-filter, php-ftp, php-gettext, php-gmp, php-hash, php-iconv, php-json, php-libxml, php-openssl, php-pcre, php-pecl-Fileinfo, php-pecl-phar, php-pecl-zip, php-reflection, php-session, php-shmop, php-simplexml   , php-sockets, php-spl, php-tokenizer, php-zend-abi, php-zip, php-zlib
    php70w-dba
    php70w-devel
    php70w-embedded     php-embedded-devel
    php70w-enchant
    php70w-fpm
    php70w-gd
    php70w-imap
    php70w-interbase        php_database, php-firebird
    php70w-intl
    php70w-ldap
    php70w-mbstring
    php70w-mcrypt
    php70w-mysql        php-mysqli, php_database
    php70w-mysqlnd      php-mysqli, php_database
    php70w-odbc     php-pdo_odbc, php_database
    php70w-opcache      php70w-pecl-zendopcache
    php70w-pdo      php70w-pdo_sqlite, php70w-sqlite3
    php70w-pdo_dblib        php70w-mssql
    php70w-pear
    php70w-pecl-apcu
    php70w-pecl-imagick
    php70w-pecl-memcached
    php70w-pecl-mongodb
    php70w-pecl-redis
    php70w-pecl-xdebug
    php70w-pgsql        php-pdo_pgsql, php_database
    php70w-phpdbg
    php70w-process      php-posix, php-sysvmsg, php-sysvsem, php-sysvshm
    php70w-pspell
    php70w-recode
    php70w-snmp
    php70w-soap
    php70w-tidy
    php70w-xml      php-dom, php-domxml, php-wddx, php-xsl
    php70w-xmlrpc
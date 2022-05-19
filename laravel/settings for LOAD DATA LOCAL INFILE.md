1. In mysql configuration `/etc/mysql/mysql.conf.d/mysqld.cnf` add line `local-infile=1`

```editor-config
[mysqld]
pid-file      = /var/run/mysqld/mysqld.pid
socket        = /var/run/mysqld/mysqld.sock
datadir       = /var/lib/mysql
log-error     = /var/log/mysql/error.log
local-infile  = 1
```

2. In laravel database configuration `config/database.php` add option `PDO::MYSQL_ATTR_LOCAL_INFILE => true`

```php
'mysql_production' => [
            'driver' => 'mysql',
            'url' => env('DATABASE_URL'),
            ...
            'options' => extension_loaded('pdo_mysql') ? array_filter([
                PDO::MYSQL_ATTR_SSL_CA => env('MYSQL_ATTR_SSL_CA'),
                PDO::MYSQL_ATTR_LOCAL_INFILE => true,
            ]) : [],
        ],
```

3. (Maybe) in php configuration file `php.ini` remove `;` at beggining of the line `mysqli.allow_local_infile = On`

```editor-config
; Allow accessing, from PHP's perspective, local files with LOAD DATA statements
; https://php.net/mysqli.allow_local_infile
mysqli.allow_local_infile = On
```

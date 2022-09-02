# cacti-debian11-installation


#apt update && apt upgrade

#apt install apache2 -y && systemctl enable --now apache2

#apt install -y php php-{mysql,curl,net-socket,gd,intl,pear,imap,memcache,pspell,tidy,xmlrpc,snmp,mbstring,gmp,json,xml,common,ldap} && apt install -y libapache2-mod-php

#nano /etc/php/*/apache2/php.ini

edit this

memory_limit = 512M
max_execution_time = 300
date.timezone = Asia/Bangkok


#nano /etc/php/*/cli/php.ini

edit this

date.timezone = Asia/Bangkok

#apt install mariadb-server -y && systemctl enable --now mariadb

#mysql -u root -p

CREATE DATABASE cacti DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci ;

GRANT ALL PRIVILEGES ON cacti.* TO 'cacti_user'@'localhost' IDENTIFIED BY 'strongpassword';

GRANT SELECT ON mysql.time_zone_name TO cacti_user@localhost;

ALTER DATABASE cacti CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

FLUSH PRIVILEGES;

EXIT;

#nano /etc/mysql/mariadb.conf.d/50-server.cnf

Add this below file
============================================
innodb_file_format=Barracuda
innodb_large_prefix=1
collation-server=utf8mb4_unicode_ci
character-set-server=utf8mb4
innodb_doublewrite=OFF
max_heap_table_size=128M
tmp_table_size=128M
join_buffer_size=256M
innodb_buffer_pool_size=2G
innodb_flush_log_at_timeout=3
innodb_read_io_threads=32
innodb_write_io_threads=16
innodb_io_capacity=5000
innodb_io_capacity_max=10000
innodb_buffer_pool_instances=9
============================================

add # tag in front of these two lines available
#character-set-server = utf8mb4
#collation-server = utf8mb4_general_ci

#mysql_tzinfo_to_sql /usr/share/zoneinfo | mysql -u root -p mysql

#apt install snmp snmpd rrdtool -y && apt install git && git clone -b 1.2.x https://github.com/Cacti/cacti.git 

#mv cacti /var/www/html

#mysql -u root cacti < /var/www/html/cacti/cacti.sql

#cd /var/www/html/cacti/include

#cp config.php.dist config.php

#nano config.php

#chown -R www-data:www-data /var/www/html/cacti

#nano /etc/systemd/system/cactid.service

add this 

[Unit]
Description=Cacti Daemon Main Poller Service
After=network.target
=================================
[Service]
Type=forking
User=www-data
Group=www-data
EnvironmentFile=/etc/default/cactid
ExecStart=/var/www/html/cacti/cactid.php
Restart=always
RestartSec=5s

[Install]
WantedBy=multi-user.target
=================================

#touch /etc/default/cactid

#systemctl daemon-reload

#systemctl enable cactid

#systemctl restart cactid

#systemctl restart apache2 mariadb


Login Cacti monitoring on Debian 11   http://your-server-IP-address/cacti/





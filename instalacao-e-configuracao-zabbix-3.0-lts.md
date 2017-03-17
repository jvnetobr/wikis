# Instalação e configuração Zabbix 3.0 LTS

wget http://repo.zabbix.com/zabbix/3.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_3.0-1+xenial_all.deb

apt-get update

apt-get install zabbix-server-pgsql zabbix-agent zabbix-frontend-php zabbix-get zabbix-sender libapache2-mod-php php-bcmath php-mbstring php-xml php-pgsql

Os pacotes abaixo serão instalados
```
apache2 apache2-bin apache2-data apache2-utils fontconfig-config fonts-dejavu-core fping libapr1 libaprutil1 libaprutil1-dbd-sqlite3 libaprutil1-ldap libcurl3 libfontconfig1 libgd3
  libiksemel3 libjbig0 libjpeg-turbo8 libjpeg8 libltdl7 liblua5.1-0 libodbc1 libopenipmi0 libpq5 libssh2-1 libtiff5 libvpx3 libxpm4 php php-common php-gd php-ldap php-mysql php7.0
  php7.0-cli php7.0-common php7.0-fpm php7.0-gd php7.0-json php7.0-ldap php7.0-mysql php7.0-opcache php7.0-readline postgresql postgresql-9.5 postgresql-client postgresql-client-9.5
  postgresql-client-common postgresql-common postgresql-contrib-9.5 ssl-cert ttf-dejavu-core
```

Editar o arquivo  "zabbix.conf", alterar "mod_php5.c" para "mod_php7.c" e alterar a linha "# php_value date.timezone Europa/Riga" para "php_value date.timezone America/Fortaleza"

vim /etc/apache2/conf-enabled/zabbix.conf




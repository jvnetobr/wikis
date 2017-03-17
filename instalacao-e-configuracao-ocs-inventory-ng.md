apt-get install make gcc

apt-get install apache2 apache2-doc 

apt-get install mysql-client

(apt-get install mysql-server)

apt-get install php php-mysql php-gd php-pclzip libapache2-mod-php

apt-get install libsoap-lite-perl

apt-get install libapache2-mod-perl2 libxml-simple-perl libapache-dbi-perl libnet-ip-perl libsoap-lite-perl libarchive-zip-perl libdbd-mysql-perl libc6-dev libxml-libxml-perl libxml-namespacesupport-perl libxml-parser-perl libxml-sax-base-perl libxml-sax-expat-perl libxml-sax-perl

apt-get install php-mbstring

wget https://github.com/OCSInventory-NG/OCSInventory-Server/archive/master.zip

unzip master.zip

cd OCSInventory-Server-master

wget https://github.com/OCSInventory-NG/OCSInventory-Server/archive/master.zip

unzip master.zip

mv OCSInventory-ocsreports-master ocsreports

rm master.zip

./setup.sh

+----------------------------------------------------------------------+
|        OK, Administration server installation finished ;-)           |
|                                                                      |
| Please, review /etc/apache2/conf-available/ocsinventory-reports.conf
|          to ensure all is good and restart Apache daemon.            |
|                                                                      |
| Then, point your browser to http://server//ocsreports
|        to configure database server and create/update schema.        |
+----------------------------------------------------------------------+


Setup has created a log file /root/OCSInventory-Server-master/ocs_server_setup.log. Please, save this file.
If you encounter error while running OCS Inventory NG Management server,
we can ask you to show us his content !

DON'T FORGET TO RESTART APACHE DAEMON !

Enjoy OCS Inventory NG ;-)

Communication server log directory - /var/log/ocsinventory-server

Communication server plugins configuration files - /etc/ocsinventory-server/plugins

Communication server plugins Perl modules files - /etc/ocsinventory-server/perl

Administration Server directory /var/lib/ocsinventory-reports
/usr/share/ocsinventory-reports


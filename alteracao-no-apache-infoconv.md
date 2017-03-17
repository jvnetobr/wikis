## É necessário realizar uma alteração no arquivo "/etc/apache2/httpd.conf" no servidor infoconv (10.10.10.103) para que as aplicações possam consumir a nova API via AJAX conforme abaixo.
`root@Infoconv:~# vim /etc/apache2/httpd.conf`
```
<VirtualHost *:80>
DocumentRoot "/var/www"
Header set Access-Control-Allow-Origin *
Header set Access-Control-Allow-Headers X-Authorization,X-Requested-With
</VirtualHost>
```
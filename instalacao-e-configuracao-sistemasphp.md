# Instalação e configuração sistemasphp

#### Realizar instalação do servidor conforme a [documentação padrão](http://gitlab.tcm.ce.gov.br/Infraestrutura/Infraestrutura/wikis/documentacao-padrao-de-instalacao)

#### Instalar apache

`root@sistemasphp-DSV:~# apt-get install apache2 apache2-bin apache2-data apache2-utils`

#### Instalar php e ferramentas

`root@sistemasphp-DSV:~# apt-get install php7.0-cli php7.0-common php7.0-curl php7.0-json php7.0-mbstring php7.0-mysql php7.0-opcache php7.0-pgsql php7.0-readline php7.0-xml php-cli-prompt php-composer-semver php-composer-spdx-licenses php-json-schema php-symfony-console php-symfony-filesystem php-symfony-finder php-symfony-process php-symfony-yaml php-xml libapache2-mod-php libapache2-mod-php7.0`

`root@sistemasphp-DSV:~# apt-get install phpunit phpunit-code-unit-reverse-lookup phpunit-comparator phpunit-diff phpunit-environment phpunit-exporter phpunit-global-state phpunit-mock-object phpunit-recursion-context phpunit-resource-operations phpunit-version`

#### Alterar arquivo `php.ini` do módulo php do apache2

`root@sistemasphp-DSV:~# vim /etc/php/7.0/apache2/php.ini`
```
upload_max_filesize = 10M
post_max_size = 10M
max_execution_time = 300
```
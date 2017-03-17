# Instalação e configuração do OpenProject no Ubuntu 14.04 por repositório.


#### Pacotes utilizados pelo OpenProject

Apache 2 (web server);

MySQL (database management system);

Unicorn (application server);

Ruby 2.1 (MRI);

#### Instalar o MySQL Server

`sudo apt-get install mysql-server libmysqlclient-dev`


#### Criar o banco de dados, criar o usuário e dar permissão no banco

`mysql -u root -p`
```
mysql> CREATE DATABASE openproject CHARACTER SET utf8;
mysql> CREATE USER 'openproject'@'localhost' IDENTIFIED BY 'senha';
mysql> GRANT ALL PRIVILEGES ON openproject.* TO 'openproject'@'localhost';
mysql> FLUSH PRIVILEGES;
mysql> QUIT
```

#### Baixar a chave GPG do repositório e adicionar ao chaveiro apt sources

`wget -qO - https://deb.packager.io/key | sudo apt-key add -`


#### Adicionar o repositório do OpenProject à lista de repositórios do Ubuntu

`echo "deb https://deb.packager.io/gh/opf/openproject-ce trusty stable/5" | sudo tee /etc/apt/sources.list.d/openproject.list`


#### Atualizar lista de pacotes

`sudo apt-get update`


#### Instalar o pacote OpenProject (o apt-get irá resolver todas as dependências necessárias para o OpenProject)

`sudo apt-get install openproject`

#### Configurar o OpenProject

`openproject configure`

![Captura_de_tela_de_2016-06-13_22-43-38](/uploads/c8fef2bfc29d70c5f0e52191ed4e58ec/Captura_de_tela_de_2016-06-13_22-43-38.png)

![Captura_de_tela_de_2016-06-13_22-43-49](/uploads/75a8b5479b0fb90b8a0dc84d83e6b75a/Captura_de_tela_de_2016-06-13_22-43-49.png)

![Captura_de_tela_de_2016-06-13_22-43-54](/uploads/4bfe1d5fe7352ced33dd4aabf9bfb7d0/Captura_de_tela_de_2016-06-13_22-43-54.png)

![Captura_de_tela_de_2016-06-13_22-43-58](/uploads/16afa7e5560a3e24f97d1bac38c9340b/Captura_de_tela_de_2016-06-13_22-43-58.png)

![Captura_de_tela_de_2016-06-13_22-44-26](/uploads/c0ba1515fcb316df9e40d3418b156414/Captura_de_tela_de_2016-06-13_22-44-26.png)

![Captura_de_tela_de_2016-06-13_22-44-30](/uploads/9034a12d3afd4c95e4f6392677c2f3b7/Captura_de_tela_de_2016-06-13_22-44-30.png)

![Captura_de_tela_de_2016-06-13_22-44-38](/uploads/cd5c0c34abf6b6252f48c8eca451dc7d/Captura_de_tela_de_2016-06-13_22-44-38.png)

![Captura_de_tela_de_2016-06-13_22-44-44](/uploads/8c552c32182faeae309849e750afc9c1/Captura_de_tela_de_2016-06-13_22-44-44.png)

![Captura_de_tela_de_2016-06-13_22-44-48](/uploads/d80d842d59df983f6cd07133b16dcf96/Captura_de_tela_de_2016-06-13_22-44-48.png)

![Captura_de_tela_de_2016-06-13_22-44-55](/uploads/8d3d6e467ce8fb4a7f8e64c51a7a53f9/Captura_de_tela_de_2016-06-13_22-44-55.png)

![Captura_de_tela_de_2016-06-13_22-44-58](/uploads/cbfb37f99c21d843c138fee8d7f6229c/Captura_de_tela_de_2016-06-13_22-44-58.png)

![Captura_de_tela_de_2016-06-13_22-45-01](/uploads/439bc34d0486e99cc9ccdd66ebe72ace/Captura_de_tela_de_2016-06-13_22-45-01.png)

![Captura_de_tela_de_2016-06-13_22-45-06](/uploads/cd294ec0ec19f2a1bc85b691f9a0fb5a/Captura_de_tela_de_2016-06-13_22-45-06.png)

![Captura_de_tela_de_2016-06-13_22-45-10](/uploads/0dbce5b8acc215a637de10de8047c748/Captura_de_tela_de_2016-06-13_22-45-10.png)

![Captura_de_tela_de_2016-06-13_22-45-12](/uploads/25d8237b038a722dce77ba49167b1bbe/Captura_de_tela_de_2016-06-13_22-45-12.png)

![Captura_de_tela_de_2016-06-13_22-45-23](/uploads/afe3e768256198a49aad2c4aed1ead47/Captura_de_tela_de_2016-06-13_22-45-23.png)

![Captura_de_tela_de_2016-06-13_22-45-30](/uploads/9dff359dc75f5d9d70f0c3d563556b26/Captura_de_tela_de_2016-06-13_22-45-30.png)

![Captura_de_tela_de_2016-06-13_22-46-37](/uploads/4130a7afc2d796d7d71e73bde3e00e13/Captura_de_tela_de_2016-06-13_22-46-37.png)

![Captura_de_tela_de_2016-06-13_22-46-40](/uploads/55b3a2e6558287a72a331ccd70bbfd57/Captura_de_tela_de_2016-06-13_22-46-40.png)

![Captura_de_tela_de_2016-06-13_22-46-52](/uploads/372a960716abd765a81d0d29fb1f3ca8/Captura_de_tela_de_2016-06-13_22-46-52.png)

![Captura_de_tela_de_2016-06-13_22-47-08](/uploads/01473a73fdb7eb170fba0ae8b700d8d4/Captura_de_tela_de_2016-06-13_22-47-08.png)

![Captura_de_tela_de_2016-06-13_22-47-12](/uploads/15fb90313e30d8c9ff1e79d38e78d191/Captura_de_tela_de_2016-06-13_22-47-12.png)

![Captura_de_tela_de_2016-06-13_22-47-30](/uploads/e45333c06080bbac08c48d077a4fbf76/Captura_de_tela_de_2016-06-13_22-47-30.png)

![Captura_de_tela_de_2016-06-13_22-47-35](/uploads/f09f5e2dc4627783380b31b7a6f99a4d/Captura_de_tela_de_2016-06-13_22-47-35.png)

#### Lista de comandos Úteis

`sudo openproject`

```
Usage:
  openproject run COMMAND [options]
  openproject scale TYPE=NUM
  openproject logs [--tail|-n NUMBER]
  openproject config:get VAR
  openproject config:set VAR=VALUE
  openproject reconfigure
```

#### Exibir configuração existente

`sudo openproject config`


#### Exibir LOG’s

`sudo openproject logs –tail`

ou

`tail -f /var/log/openproject/production.log
tail -f /var/log/openproject/web-1.log
tail -f /var/log/openproject/worker-1.log`


#### Caso seja necessário realizar uma reconfiguração

`sudo openproject reconfigure`

#### Atualizar para uma nova versão

`sudo apt-get update
sudo apt-get install --only-upgrade openproject
sudo openproject configure`



# Backup e Restore banco de dados MySQL

#### Este tutorial irá mostrar maneiras fáceis de realizar o backup e restaurar os dados em seu banco de dados MySQL.

### Realizando backup do terminal utilizando o mysqldump

#### Iremos utilizar o comando "mysqldump" para realizar o backup dos dados do MySQL. Este comando se conecta ao servidor MySQL e cria um arquivo de despejo do SQL. O arquivo de despejo contém as instruções SQL necessárias para recriar o banco de dados. Veja abaixo a sintaxe utilizada:

`$ mysqldump -u [usuario] -p[senha] [dbname] > [backupfile.sql]`

* [usuario] Usuário do banco de dados
* [senha] A senha do usuário do banco de dados (não há espaço entre "-p" e a senha)
* [dbname] O nome do banco de dados
* [backupfile.sql] O nome do backup do banco de dados

#### Por exemplo, para fazer o backup do banco de dados "teste" com usuário "root" para um arquivo "teste_bkp.sql", você poderá utilizar o comando abaixo:

`$ mysqldump -u root -p teste > teste_bkp.sql`

#### Este comando irá fazer backup do banco de dados "teste" em um arquivo chamado "teste_bkp.sql" que conterá todas as instruções SQL necessárias para recriar o banco de dados.

#### Com o comando mysqldump você pode especificar determinadas tabelas do banco de dados que deseja fazer backup. Por exemplo, para fazer backup somente de tabelas php_teste e asp_teste da base de dados "teste" realize o comando abaixo. Cada nome de tabela tem de ser separado por espaço.

`$ mysqldump -u root -p teste php_teste asp_teste > teste_bkp.sql`

#### Às vezes, é necessário fazer backup de mais de um banco de dados ao mesmo tempo. Neste caso, você pode usar a opção "--databases" seguida pela lista de bancos de dados que você gostaria de fazer backup. Cada nome de banco de dados deve ser separado por espaço.

`$ mysqldump -u root -p --databases teste tutoriais artigos > bancos_bkp.sql`

#### Se você quiser fazer backup de todos os bancos de dados no servidor ao mesmo tempo, use a opção "--all-databases". Ele diz ao MySQL para realizar o dump de todos os bancos de dados existentes para o destino.

`$ mysqldump -u root -p --all-databases > todosbancos_bkp.sql`

#### O comando mysqldump tem também algumas outras opções úteis:

--add-drop-table: Diz ao MySQL para adicionar uma instrução DROP TABLE antes de cada CREATE TABLE no dump.

--no-data: Realiza o dump somente da estrutura do banco de dados, não o conteúdo.

--add-locks: Adiciona as instruções LOCK TABLES e UNLOCK TABLES que você pode ver no arquivo de dump.

#### O comando mysqldump tem vantagens e desvantagens. As vantagens de usar mysqldump são que é simples de usar e cuida de questões de bloqueio de tabela para você. A desvantagem é que o comando bloqueia tabelas. Se o tamanho das suas tabelas é muito grande mysqldump pode bloquear os usuários por um longo período de tempo.

### Realizando backup do banco de dados com compactação

#### Se o seu banco de dados mysql é muito grande, você pode querer compactar a saída do mysqldump. Basta usar o comando de backup mysql abaixo e canalizar a saída para gzip, então você terá a saída como arquivo gzip.

`$ mysqldump -u [usuario] -p[senha] [dbname] | gzip -9 > [backupfile.sql.gz]`

#### Se você quiser extrair o arquivo .gz, use o comando abaixo:

`$ gunzip [backupfile.sql.gz]`

### Realizando restore do banco de dados

#### Acima realizamos o backup do banco de dados "teste" no arquivo "teste_bkp.sql". Para recriar o banco de dados "teste" você deve seguir duas etapas:

* Crie um banco de dados com o mesmo nome na máquina de destino
* Carregue o arquivo usando o comando mysql:

`$ mysql -u root -p teste < teste_bkp.sql`

#### Para restaurar arquivos de backup compactados, você pode fazer o seguinte:

`$ gunzip < [backupfile.sql.gz] | mysql -u [usuario] -p[senha] [dbname]`

#### Se você precisar restaurar um banco de dados que já existe, você precisará usar o comando mysqlimport. A sintaxe para mysqlimport é a seguinte:

`$ mysqlimport -u [usuario] -p[senha] [dbname] [backupfile.sql]`

#### Se você precisar restaurar todos os bancos de dados, segue abaixo a sintaxe. Para esse comando, os bancos de dados precisam existir ou o script "todosbancos_bkp.sql" precisa conter os parâmetros CREATE DATABASE para os bancos de dados.
`$ mysql -u root -p < todosbancos_bkp.sql`
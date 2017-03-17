# Administração de usuários no MySQL

Esta documentação foi feita baseada na documentação oficial do [MySQL](http://dev.mysql.com/doc/) para versão 5.7

## Criação de usuários

#### Normalmente, um administrador de banco de dados primeiro usa CREATE USER para criar uma conta e definir suas características não prioritárias, como sua senha, se usa conexões seguras e limita o acesso aos recursos do servidor, então usa GRANT para definir seus privilégios. ALTER USER pode ser usado para alterar as características não prioritárias de contas existentes. Por exemplo:

```
CREATE USER 'jeffrey'@'localhost' IDENTIFIED BY 'mypass';
GRANT ALL ON db1.* TO 'jeffrey'@'localhost';
GRANT SELECT ON db2.invoice TO 'jeffrey'@'localhost';
ALTER USER 'jeffrey'@'localhost' WITH MAX_QUERIES_PER_HOUR 90;
```

## Exclusão de usuários

#### A instrução DROP USER remove uma ou mais contas MySQL e seus privilégios. Ele remove linhas de privilégios para a conta de todas as tabelas de concessão.

#### Para usar DROP USER, você deve ter o privilégio global CREATE USER ou o privilégio DELETE para o banco de dados mysql. Quando a variável de sistema read_only está habilitada, DROP USER requer, adicionalmente, o privilégio SUPER.

#### Ocorre um erro se você tentar soltar uma conta que não existe.

#### A partir do MySQL 5.7.8, a cláusula IF EXISTS pode ser usada, o que faz com que a declaração produza um aviso para cada conta nomeada que não existe, em vez de um erro.

```
DROP USER 'jeffrey'@'localhost';
```

Obs.: A parte do nome do host do nome da conta, se omitida, assume como padrão '%'.

DROP USER não descarta ou invalida automaticamente bancos de dados ou objetos dentro deles que o usuário antigo criou. Isso inclui programas armazenados ou modos de exibição para os quais o atributo DEFINER nomeia o usuário descartado. As tentativas de acessar esses objetos podem produzir um erro se eles forem executados no contexto de segurança do definidor.

## Exemplos de Permissões

### Privilégios globais

#### Privilégios globais são administrativos ou aplicam-se a todos os bancos de dados em um determinado servidor. Para atribuir privilégios globais, use a sintaxe ON *. *:

```
GRANT ALL ON *.* TO 'someuser'@'somehost';
GRANT SELECT, INSERT ON *.* TO 'someuser'@'somehost';
```

Os privilégios CREATE TABLESPACE, CREATE USER, FILE, PROCESS, RELOAD, REPLICATION CLIENT, REPLICATION SLAVE, SHOW DATABASES, SHUTDOWN e SUPER são administrativos e só podem ser concedidos globalmente.

Outros privilégios podem ser concedidos globalmente ou em níveis mais específicos.

O MySQL armazena privilégios globais na tabela mysql.user.

### Privilégios de banco de dados

#### Os privilégios de banco de dados aplicam-se a todos os objetos em um determinado banco de dados. Para atribuir privilégios de nível de banco de dados, use a sintaxe ON db_name.* :

```
GRANT ALL ON mydb.* TO 'someuser'@'somehost';
GRANT SELECT, INSERT ON mydb.* TO 'someuser'@'somehost';
```

Se você usar a sintaxe ON * (em vez de ON *. *) E tiver selecionado um banco de dados padrão, os privilégios serão atribuídos no nível do banco de dados padrão para o banco de dados. Ocorre um erro se não houver nenhum banco de dados padrão.

Os privilégios CREATE, DROP, EVENT, GRANT OPTION, LOCK TABLES e REFERENCES podem ser especificados no nível do banco de dados. Os privilégios de tabela ou de rotina também podem ser especificados no nível do banco de dados, caso em que eles se aplicam a todas as tabelas ou rotinas do banco de dados.

O MySQL armazena os privilégios do banco de dados na tabela mysql.db.

### Privilégios de tabela

#### Os privilégios de tabela aplicam-se a todas as colunas de uma determinada tabela. Para atribuir privilégios ao nível da tabela, utilize ON sintaxe db_name.tbl_name:

```
GRANT ALL ON mydb.mytbl TO 'someuser'@'somehost';
GRANT SELECT, INSERT ON mydb.mytbl TO 'someuser'@'somehost';
```

Se você especificar tbl_name em vez de db_name.tbl_name, a instrução se aplica a tbl_name no banco de dados padrão. Ocorre um erro se não houver nenhum banco de dados padrão.

Os valores de priv_type permitidos no nível da tabela são ALTER, CREATE VIEW, CREATE, DELETE, DROP, GRANT OPTION, INDEX, INSERT, REFERENCES, SELECT, SHOW VIEW, TRIGGER e UPDATE.

O MySQL armazena privilégios de tabelas na tabela mysql.tables_priv.

### Privilégios de coluna

#### Os privilégios de colunas aplicam-se a colunas individuais em uma determinada tabela. Cada privilégio a ser concedido no nível da coluna deve ser seguido pela coluna ou colunas, entre parênteses.

```
GRANT SELECT (col1), INSERT (col1,col2) ON mydb.mytbl TO 'someuser'@'somehost';
```

Os valores priv_type permissíveis para uma coluna (ou seja, quando você usa uma cláusula column_list) são INSERT, REFERENCES, SELECT e UPDATE.

O MySQL armazena privilégios de colunas na tabela mysql.columns_priv.

### Privilégios de rotina armazenados

#### Os privilégios ALTER ROUTINE, CREATE ROUTINE, EXECUTE e GRANT OPTION aplicam-se a rotinas armazenadas (procedimentos e funções). Eles podem ser concedidos nos níveis global e de banco de dados. Exceto para CREATE ROUTINE, esses privilégios podem ser concedidos no nível de rotina para rotinas individuais.

```
GRANT CREATE ROUTINE ON mydb.* TO 'someuser'@'somehost';
GRANT EXECUTE ON PROCEDURE mydb.myproc TO 'someuser'@'somehost';
```

Os valores de "priv_type" permitidos no nível de rotina são ALTER ROUTINE, EXECUTE e GRANT OPTION. CREATE ROUTINE não é um privilégio de nível de rotina porque você deve ter esse privilégio para criar uma rotina em primeiro lugar.

O MySQL armazena privilégios de nível de rotina na tabela mysql.procs_priv.

### Privilégios do usuário do proxy

#### O privilégio PROXY permite que um usuário seja um proxy para outro. O usuário proxy personifica ou toma a identidade do usuário proxy.

```
GRANT PROXY ON 'localuser'@'localhost' TO 'externaluser'@'somehost';
```

Quando PROXY é concedido, ele deve ser o único privilégio nomeado na instrução GRANT, a cláusula REQUIRE não pode ser dada ea única opção WITH permitida é WITH GRANT OPTION.

Proxy requer que o usuário proxy autenticar por meio de um plugin que retorna o nome do usuário proxy para o servidor quando o usuário proxy se conecta e que o usuário proxy tem o privilégio PROXY para o usuário proxy.

O MySQL armazena privilégios de proxy na tabela mysql.proxies_priv.

### Nomes de conta e senhas

#### Um valor de usuário em uma instrução GRANT indica uma conta MySQL à qual a declaração se aplica. Para acomodar a concessão de direitos a usuários de hosts arbitrários, o MySQL suporta especificar o valor do usuário no formato 'user_name' @ 'host_name'.

#### Você pode especificar curingas no nome do host. Por exemplo, 'user_name'@'%.example.com' se aplica a user_name para qualquer host no domínio example.com, e 'user_name'@'192.168.1.%' aplica-se a user_name para qualquer host no 192.168.1 Classe C sub-rede.

#### O formulário simples 'user_name' é um sinônimo de 'user_name' @ '%'.

#### O MySQL não suporta curingas em nomes de usuário. Para se referir a um usuário anônimo, especifique uma conta com um nome de usuário vazio com a instrução GRANT:

```
GRANT ALL ON test.* TO ''@'localhost' ...;
```

Neste caso, qualquer usuário que se conectar do host local com a senha correta para o usuário anônimo terá acesso permitido, com os privilégios associados à conta de usuário anônimo.

### Criação Implícita de Conta

#### Se uma conta nomeada em uma instrução GRANT não existir, a ação tomada depende do modo SQL NO_AUTO_CREATE_USER:

#### * Se NO_AUTO_CREATE_USER não estiver habilitado, GRANT cria a conta. Isso é muito inseguro, a menos que você especifique uma senha não vazia usando IDENTIFIED BY.

#### * Se NO_AUTO_CREATE_USER estiver ativado, GRANT falhará e não criará a conta, a menos que você especifique uma senha não vazia usando IDENTIFIED BY ou nomeie um plug-in de autenticação usando IDENTIFIED WITH.

#### * A partir do MySQL 5.7.2, se a conta já existir, IDENTIFIED WITH é proibido porque é destinado somente para uso ao criar novas contas.

## Remover privilégios

#### A declaração REVOKE permite aos administradores de sistema revogar privilégios de contas MySQL.

#### Quando a variável de sistema read_only está habilitada, REVOKE requer o privilégio SUPER além de quaisquer outros privilégios necessários descritos na discussão a seguir.

```
REVOKE INSERT ON *.* FROM 'jeffrey'@'localhost';
```
Obs.: A parte do nome do host do nome da conta, se omitida, assume como padrão '%'.

#### Para usar a primeira sintaxe REVOKE, você deve ter o privilégio GRANT OPTION e você deve ter os privilégios que você está revogando.

#### Para revogar todos os privilégios, use a segunda sintaxe, que descarta todos os privilégios globais, de banco de dados, de tabela, de coluna e de rotina para o usuário ou usuários nomeados:
```
REVOKE ALL PRIVILEGES, GRANT OPTION FROM user [, user] ...
```

Para usar essa sintaxe REVOKE, você deve ter o privilégio global CREATE USER ou UPDATE para o banco de dados mysql.

Se as tabelas de concessão contiverem linhas de privilégios que contêm nomes de banco de dados de misto ou de tabela e a variável de sistema lower_case_table_names estiver definida como um valor diferente de zero, REVOKE não pode ser usado para revogar esses privilégios. Será necessário manipular as tabelas de concessão diretamente. (GRANT não criará tais linhas quando lower_case_table_names estiver definido, mas essas linhas poderiam ter sido criadas antes de definir a variável.)

Quando executado com êxito a partir do programa mysql, REVOKE responde com Query OK, 0 linhas afetadas. Para determinar quais privilégios resultam da operação, use SHOW GRANTS.
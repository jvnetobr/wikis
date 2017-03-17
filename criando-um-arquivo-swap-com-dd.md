# Criando, habilitando e ajustando o uso da swap

Para criar o nosso arquivo de Swap deveremos utilizar os parâmetros `bs`(tamanho do bloco) e `count`(número de vezes que o bloco será copiado). Por padrão o parâmetro `bs` é definido em Bytes,  mas podemos adicionar também as letras K (kilobyte), M (megabyte) e G (gigabyte) junto aos valores dos blocos.

#### Vamos verificar quanto o nosso sistema tem de swap
`root@vm-testes:~# free -h`

## Criando o arquivo de swap

#### Vamos criar abaixo um arquivo de 4GB para nossa swap
`root@vm-testes:~# dd if=/dev/zero of=/swapfile bs=1G count=4`

#### Mude as permissões do novo arquivo criado para leitura e gravação somente para o dono
`root@vm-testes:~# chmod 0600 /swapfile`

#### Formate o arquivo de swap
`root@vm-testes:~# mkswap /swapfile`

## Habilitando o arquivo de swap

#### Podemos habilitar o arquivo de swap imediatamente e habilitar na inicialização do sistema
Para habilitar imediatamente:

`root@vm-testes:~# swapon /swapfile`

Para habilitar na inicialização do sistema edite o arquivo `/etc/fstab` adicionando a entrada abaixo

`root@vm-testes:~# vim /etc/fstab`
```
/swapfile          swap            swap    defaults        0 0
```

#### Depois de criar e habilitar o arquivo de swap, vamos verificar se ela está habilitada
`root@vm-testes:~# free -h`

ou

`root@vm-testes:~# swapon -s`

## Ajustando a propriedade `swappiness`

O parâmetro `swappiness` configura como será feita a troca de dados da memória RAM para a Swap. O valor pode ser definido entre 0 e 100. Com o valor "0", o kernel não vai utilizar a Swap, a não ser que o uso da memória RAM chegue a 100%. Com o valor "10", a Swap somente vai ser usada quando o uso da memória RAM chegar a 90%. Com o valor próximo a 100, o kernel vai priorizar o uso da Swap, podendo deixar mais espaço livre na RAM. Esse perfil de utilização da memória deve ser definido dependendo do uso das aplicações no servidor. 

Lembrando que priorizando o uso da Swap poderá causar uma redução significativa no desempenho do sistema, no contrário priorizando o uso da memória RAM geralmente deixa o sistema mais rápido.

#### Podemos visualizar o valor `swappiness` com os comandos abaixo:

`root@vm-testes:~# cat /proc/sys/vm/swappiness`

Ou

`root@vm-testes:~# sysctl -a | grep swappiness`

#### Vamos definir o valor como 10

`root@vm-testes:~# echo 'vm.swappiness = 10' >> /etc/sysctl.conf`

## Ajustando a gestão de cache

O parâmetro `vfs_cache_pressure` define como o kernel remove as informações de inode (estrutura de dados usada para representar um objeto do sistema de arquivos) do cache do sistema de arquivos. Quanto menor for o valor definido, menor será o uso do cache e quanto maior for o valor, mais lento o sistema pode ficar. 

#### O valor padrão é "100", vamos definir como "50" para um cenário mais conservador
`root@vm-testes:~# echo 'vm.vfs_cache_pressure = 50' >> /etc/sysctl.conf`

#### Vamos atualizar a configuração do kernel
`root@vm-testes:~# sysctl -p`

# Limpeza da partição "/boot"

Quando criada a partição "/boot" na instalação do sistema operacional GNU/Linux, geralmente é criada com 500MB de tamanho. Esse valor pode chegar a lotar com o decorrer do tempo e de atualizações nas versões do kernel Linux. Existem muitos tutoriais que recomendam remover todos os pacotes que não sejam a versão atual do kernel, porém é interessante manter ao menos a versão anterior do kernel. 

#### Primeiramente vamos exibir as informações da partição "/boot"
`root@vm-testes:~# df -h /boot`
```
Sist. Arq.      Tam. Usado Disp. Uso% Montado em
/dev/xvda1                               472M  423M   25M  95% /boot
```
#### Vamos utilizar o comando abaixo para listar os pacotes "linux-headers" e "linux-image" sem listar o kernel utilizado atualmente no Ubuntu
`root@vm-testes:~# dpkg -l linux-{image,headers}-"[0-9]*" | awk '/ii/{print $2}' | grep -ve "$(uname -r | sed -r 's/-[a-z]+//')"`
```
linux-headers-4.4.0-24
linux-headers-4.4.0-24-generic
linux-headers-4.4.0-28
linux-headers-4.4.0-28-generic
linux-headers-4.4.0-31
linux-headers-4.4.0-31-generic
linux-headers-4.4.0-34
linux-headers-4.4.0-34-generic
linux-headers-4.4.0-36
linux-headers-4.4.0-36-generic
linux-headers-4.4.0-38
linux-headers-4.4.0-38-generic
linux-image-4.4.0-21-generic
linux-image-4.4.0-22-generic
linux-image-4.4.0-24-generic
linux-image-4.4.0-28-generic
linux-image-4.4.0-31-generic
linux-image-4.4.0-34-generic
linux-image-4.4.0-36-generic
linux-image-4.4.0-38-generic
```

#### Vamos remover somente o primeiro pacote listado
`root@vm-testes:~# apt-get purge linux-headers-4.4.0-24`

#### Observe que o sistema exibe uma lista de pacotes que foram instalados automaticamente e já não são necessários
```
Lendo listas de pacotes... Pronto
Construindo árvore de dependências       
Lendo informação de estado... Pronto
Os seguintes pacotes foram instalados automaticamente e já não são necessários:
  linux-headers-4.4.0-28 linux-headers-4.4.0-28-generic linux-headers-4.4.0-31 linux-headers-4.4.0-31-generic linux-headers-4.4.0-34 linux-headers-4.4.0-34-generic linux-headers-4.4.0-36
  linux-headers-4.4.0-36-generic linux-image-4.4.0-21-generic linux-image-4.4.0-22-generic linux-image-4.4.0-24-generic linux-image-4.4.0-28-generic linux-image-4.4.0-31-generic
  linux-image-4.4.0-34-generic linux-image-4.4.0-36-generic linux-image-extra-4.4.0-21-generic linux-image-extra-4.4.0-22-generic linux-image-extra-4.4.0-24-generic
  linux-image-extra-4.4.0-28-generic linux-image-extra-4.4.0-31-generic linux-image-extra-4.4.0-34-generic linux-image-extra-4.4.0-36-generic
Utilize 'apt autoremove' para os remover.
Os pacotes a seguir serão REMOVIDOS:
  linux-headers-4.4.0-24* linux-headers-4.4.0-24-generic*
0 pacotes atualizados, 0 pacotes novos instalados, 2 a serem removidos e 68 não atualizados.
Depois desta operação, 77,5 MB de espaço em disco serão liberados.
Você quer continuar? [S/n] s
(Lendo banco de dados ... 270783 ficheiros e directórios actualmente instalados.)
A remover linux-headers-4.4.0-24-generic (4.4.0-24.43) ...
A remover linux-headers-4.4.0-24 (4.4.0-24.43) ...
```

#### Agora vamos utilizar o comando que o sistema nos recomenda
`root@vm-testes:~# apt autoremove`
```
Lendo listas de pacotes... Pronto
Construindo árvore de dependências       
Lendo informação de estado... Pronto
Os pacotes a seguir serão REMOVIDOS:
  linux-headers-4.4.0-28 linux-headers-4.4.0-28-generic linux-headers-4.4.0-31 linux-headers-4.4.0-31-generic linux-headers-4.4.0-34 linux-headers-4.4.0-34-generic linux-headers-4.4.0-36
  linux-headers-4.4.0-36-generic linux-image-4.4.0-21-generic linux-image-4.4.0-22-generic linux-image-4.4.0-24-generic linux-image-4.4.0-28-generic linux-image-4.4.0-31-generic
  linux-image-4.4.0-34-generic linux-image-4.4.0-36-generic linux-image-extra-4.4.0-21-generic linux-image-extra-4.4.0-22-generic linux-image-extra-4.4.0-24-generic
  linux-image-extra-4.4.0-28-generic linux-image-extra-4.4.0-31-generic linux-image-extra-4.4.0-34-generic linux-image-extra-4.4.0-36-generic
0 pacotes atualizados, 0 pacotes novos instalados, 22 a serem removidos e 68 não atualizados.
Depois desta operação, 1.833 MB de espaço em disco serão liberados.
Você quer continuar? [S/n] s
```

#### Após concluir execute novamente o comando para listar os pacotes do kernel
`root@vm-testes:~# dpkg -l linux-{image,headers}-"[0-9]*" | awk '/ii/{print $2}' | grep -ve "$(uname -r | sed -r 's/-[a-z]+//')"`
```
linux-headers-4.4.0-38
linux-headers-4.4.0-38-generic
linux-image-4.4.0-38-generic
```

#### Dependendo das versões instaladas, você deverá ter uma saída similar à exibida acima, contendo os pacotes da versão anterior do kernel. Para confirmar o espaço livre na partição "/boot" vamos executar novamente o comando "df"
`root@vm-testes:~# df -h /boot`
```
Sist. Arq.      Tam. Usado Disp. Uso% Montado em
/dev/xvda1      472M  101M  348M  23% /boot
```

#### Pronto! Nossa partição "/boot" já está com mais espaço livre disponível.
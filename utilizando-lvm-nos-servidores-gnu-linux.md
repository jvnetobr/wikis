# Utilizando LVM nos servidores GNU/Linux

### 1. Criando partição LVM

Vamos localizar o disco de 50 GB adicionado recentemente à VM

`root@vm-testes:~# fdisk -l`
```
...
Disk /dev/sdb: 50 GiB, 53687091200 bytes, 104857600 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
...
```
Vamos utilizar o disco "/dev/sdb" como exemplo

`root@vm-testes:~# fdisk /dev/sdb`

Vamos criar uma nova partição

```
Comando (m para ajuda): n
```
Escolha o tipo de partição
```
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
```
Utilizando o sistema de particionamento MBR, somente é possível as quatro primeiras partições de um disco serem primárias.
```
Select (default p): p
```
Esta será a primeira partição
```
Partition number (1-4, default 1): 1
```
O fdisk agora pergunta qual setor a partição deverá iniciar e em Agora o fdisk me pergunta em qual setor essa partição deve começar e em seguida o último setor, definindo o tamanho, como iremos utilizar todo o disco vamos escolher as opções "default" com a tecla ENTER.
```
First sector (2048-104857599, default 2048): 
Last sector, +sectors or +size{K,M,G,T,P} (2048-104857599, default 104857599): 

Created a new partition 1 of type 'Linux' and of size 50 GiB.
```
Vamos listar as partições
```
Comando (m para ajuda): p

Disk /dev/sdb: 64.4 GB, 64424509440 bytes
255 heads, 63 sectors/track, 7832 cylinders, total 125829120 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0xe61fc21b

   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048   125829119    62913536   83  Linux
```
Vamos escolher o tipo da partição
```
Comando (m para ajuda): t

Selected partition 1
Partition type (type L to list all types): 8e
Changed type of partition 'Linux' to 'Linux LVM'.
```
Vamos gravar as alterações no disco
```
Comando (m para ajuda): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```
Vamos listar agora a tabela de partições do disco "/dev/sdb" e veja a criação da partição "/dev/sdb1"

`root@vm-testes:~# fdisk -l /dev/sdb`
```
Disk /dev/sdb: 50 GiB, 53687091200 bytes, 104857600 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x5673f5b0

Dispositivo Inicializar Start       Fim   Setores Size Id Tipo
/dev/sdb1                2048 104857599 104855552  50G 8e Linux LVM
```

### 2. Criando PV (Physical Volume)

Após criarmos uma partição LVM, vamos criar um PV

`root@vm-testes:~# pvcreate /dev/sdb1`
```
  Physical volume "/dev/sdb1" successfully created
```
Vamos escanear os PVs e perceba que o "/dev/sdb1" foi criado conforme exibe abaixo. 

`root@vm-testes:~# pvscan`
```
  PV /dev/sda5   VG ubuntu-testvm-vg   lvm2 [24,52 GiB / 32,00 MiB free]
  PV /dev/sdb1                         lvm2 [50,00 GiB]
  Total: 2 [74,52 GiB] / in use: 1 [24,52 GiB] / in no VG: 1 [50,00 GiB]
```
Para maiores detalhes do PV criado utilize o comando "pvdisplay"

`root@vm-testes:~# pvdisplay /dev/sdb1`
```
  "/dev/sdb1" is a new physical volume of "50,00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/sdb1
  VG Name               
  PV Size               50,00 GiB
  Allocatable           NO
  PE Size               0   
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               spINyj-nfj0-J3Tb-oprJ-ExVl-WSlO-DkVqk3
   
```
### 3. Criando um VG (Volume Group)

Vamos criar o VG de exemplo "vg-teste" conforme abaixo

`root@vm-testes:~# vgcreate vg-teste /dev/sdb1`
```
  Volume group "vg-teste" successfully created
```
Vamos escanear os VGs e perceba que o "vg-teste" foi criado conforme exibe abaixo. 

`root@vm-testes:~# vgscan`
```
  Reading all physical volumes.  This may take a while...
  Found volume group "vg-teste" using metadata type lvm2
  Found volume group "ubuntu-testvm-vg" using metadata type lvm2
```
Para maiores detalhes do VG criado utilize o comando "vgdisplay"

`root@vm-testes:~# vgdisplay vg-teste`
```
  --- Volume group ---
  VG Name               vg-teste
  System ID             
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               50,00 GiB
  PE Size               4,00 MiB
  Total PE              12799
  Alloc PE / Size       0 / 0   
  Free  PE / Size       12799 / 50,00 GiB
  VG UUID               eZ2jyt-A8tb-e1Lo-bLhn-VYcG-H8qF-ClDMco
   
```

### 4. Adicionando PV (Physical Volume) ao VG (Volume Group)

Vamos adicionar o PV "/dev/sdc1" ao VG "vg-teste" usando o "vgextend" conforme abaixo

`root@vm-testes:~# vgextend vg-teste /dev/sdc1`
```
  Volume group "vg-teste" successfully extended
```

### 5. Criando LV (Logical Volume)

Vamos criar agora o LV "lvubuntu" no VG "vg-teste"

`root@vm-testes:~# lvcreate --name lvubuntu --size 40G vg-teste`

Ou para criar com 100% do espaço livre do VG

`root@vm-testes:~# lvcreate -l 100%FREE -n lvubuntu vg-teste`
```
  Logical volume "lvubuntu" created.
```
Vamos escanear os LVs e perceba que o "lvubuntu" foi criado conforme exibe abaixo. 

`root@vm-testes:~# lvscan`
```
  ACTIVE            '/dev/vg-teste/lvubuntu' [40,00 GiB] inherit
  ACTIVE            '/dev/ubuntu-testvm-vg/root' [23,74 GiB] inherit
  ACTIVE            '/dev/ubuntu-testvm-vg/swap_1' [764,00 MiB] inherit
```
Para maiores detalhes do LV criado no "vg-teste" utilize o comando "lvdisplay"

`root@vm-testes:~# lvdisplay vg-teste`
```
  --- Logical volume ---
  LV Path                /dev/vg-teste/lvubuntu
  LV Name                lvubuntu
  VG Name                vg-teste
  LV UUID                C8vdEw-oDV3-MfN4-Lb0V-azZa-Mhhe-X2e9rc
  LV Write Access        read/write
  LV Creation host, time web, 2016-08-10 10:46:51 -0300
  LV Status              available
  # open                 0
  LV Size                40,00 GiB
  Current LE             10240
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           252:2
   
```
### 6. Formatando a partição LVM

Vamos agora formatar a partição LVM

`root@vm-testes:~# mkfs.ext4 /dev/mapper/vg--teste-lvubuntu`
```
mke2fs 1.42.13 (17-May-2015)
Creating filesystem with 10485760 4k blocks and 2621440 inodes
Filesystem UUID: aa2a4d29-554d-4dea-9ce8-bacec86a6b5f
Cópias de segurança de superblocos gravadas em blocos: 
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
	4096000, 7962624

Allocating group tables: pronto                            
Gravando tabelas inode: pronto                            
Creating journal (32768 blocks): concluído
Escrevendo superblocos e informações de contabilidade de sistema de arquivos: concluído

```

### 7. Montando a partição LVM

Vamos criar o diretório que a partição será montada e realizar a montagem

`root@vm-testes:~# mkdir -p /dados/lvubuntu`

`root@vm-testes:~# mount /dev/mapper/vg--teste-lvubuntu /dados/lvubuntu/`

### 8. Configurando fstab

Vamos utilizar o "blkid" para listar o "UUID" da partição

`root@vm-testes:~# blkid | grep "lvubuntu"`
```
/dev/mapper/vg--teste-lvubuntu: UUID="aa2a4d29-554d-4dea-9ce8-bacec86a6b5f" TYPE="ext4"
```
Vamos adicionar a linha abaixo como uma entrada no arquivo "/etc/fstab"

`root@vm-testes:~# vim /etc/fstab`
```
UUID=aa2a4d29-554d-4dea-9ce8-bacec86a6b5f /dados/lvubuntu               ext4    defaults 0       1
```
### 9. Expandindo o LV (Logical Volume)

Vamos desmontar o LV que será estendido

`root@vm-testes:~# umount /dados/lvubuntu`

Vamos estender o LV com mais 5 GB

`root@vm-testes:~# lvextend -L +5GB /dev/mapper/vg--teste-lvubuntu`
```
  Size of logical volume vg-teste/lvubuntu changed from 40,00 GiB (10240 extents) to 45,00 GiB (11520 extents).
  Logical volume lvubuntu successfully resized.
```
Vamos verificar o LV executando o "e2fsck"

`root@vm-testes:~# e2fsck -f /dev/mapper/vg--teste-lvubuntu`
```
e2fsck 1.42.13 (17-May-2015)
Passo 1: Verificando inodes, blocks, e os tamanhos.
Passo 2: Verificando estrutura directory
Passo 3: Checando conectividade com o directory
Passo 4: Checando contagens de referência
Passo 5: Procurando informações de resumo group
/dev/mapper/vg--teste-lvubuntu: 11/2621440 files (0.0% non-contiguous), 209554/10485760 blocks
```

Vamos agora redimensionar a partição

`root@vm-testes:~# resize2fs -p /dev/mapper/vg--teste-lvubuntu`
```
resize2fs 1.42.13 (17-May-2015)
Resizing the filesystem on /dev/mapper/vg--teste-lvubuntu to 11796480 (4k) blocks.
The filesystem on /dev/mapper/vg--teste-lvubuntu is now 11796480 (4k) blocks long.
```
Vamos novamente executar o "e2fsck", porém agora para verificar a partição redimensionada.

`root@vm-testes:~# e2fsck -f /dev/mapper/vg--teste-lvubuntu`
```
e2fsck 1.42.13 (17-May-2015)
Passo 1: Verificando inodes, blocks, e os tamanhos.
Passo 2: Verificando estrutura directory
Passo 3: Checando conectividade com o directory
Passo 4: Checando contagens de referência
Passo 5: Procurando informações de resumo group
/dev/mapper/vg--teste-lvubuntu: 11/2949120 files (0.0% non-contiguous), 231139/11796480 blocks
```
Vamos montar novamente a partição

`root@vm-testes:~# mount /dev/mapper/vg--teste-lvubuntu /dados/lvubuntu/`

Vamos checar o tamanho da nova partição montada

`root@vm-testes:~# df -kh /dev/mapper/vg--teste-lvubuntu`
```
Sist. Arq.                      Tam. Usado Disp. Uso% Montado em
/dev/mapper/vg--teste-lvubuntu   45G   52M   42G   1% /dados/lvubuntu
```
### 10. Diminuindo o LV (Logical Volume)

Vamos desmontar o LV que será reduzido

`root@vm-testes:~# umount /dados/lvubuntu`

Vamos verificar a partição "/dev/mapper/vg--teste-lvubuntu" primeiramente

`root@vm-testes:~# e2fsck -f /dev/mapper/vg--teste-lvubuntu`
```
e2fsck 1.42.13 (17-May-2015)
Passo 1: Verificando inodes, blocks, e os tamanhos.
Passo 2: Verificando estrutura directory
Passo 3: Checando conectividade com o directory
Passo 4: Checando contagens de referência
Passo 5: Procurando informações de resumo group
/dev/mapper/vg--teste-lvubuntu: 11/2949120 files (0.0% non-contiguous), 231139/11796480 blocks
```
Vamos reduzir o tamanho da partição para 40 GB

`root@vm-testes:~# resize2fs -p /dev/mapper/vg--teste-lvubuntu 40G`
```
resize2fs 1.42.13 (17-May-2015)
Resizing the filesystem on /dev/mapper/vg--teste-lvubuntu to 10485760 (4k) blocks.
Iniciar passo 3 (máximo = 360)
Varrendo tabela de inodes     XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
The filesystem on /dev/mapper/vg--teste-lvubuntu is now 10485760 (4k) blocks long.
```
Vamos agora reduzir o tamanho do LV para 40 GB e confirmar com "y"

`root@vm-testes:~# lvreduce -L 40G /dev/mapper/vg--teste-lvubuntu`
```
  WARNING: Reducing active logical volume to 40,00 GiB
  THIS MAY DESTROY YOUR DATA (filesystem etc.)
Do you really want to reduce lvubuntu? [y/n]: y
  Size of logical volume vg-teste/lvubuntu changed from 45,00 GiB (11520 extents) to 40,00 GiB (10240 extents).
  Logical volume lvubuntu successfully resized.
```
Vamos verificar a partição "/dev/mapper/vg--teste-lvubuntu" novamente

`root@vm-testes:~# e2fsck -f /dev/mapper/vg--teste-lvubuntu`
```
e2fsck 1.42.13 (17-May-2015)
Passo 1: Verificando inodes, blocks, e os tamanhos.
Passo 2: Verificando estrutura directory
Passo 3: Checando conectividade com o directory
Passo 4: Checando contagens de referência
Passo 5: Procurando informações de resumo group
/dev/mapper/vg--teste-lvubuntu: 11/2621440 files (0.0% non-contiguous), 209554/10485760 blocks
```
Vamos montar a partição novamente

`root@vm-testes:~# mount /dados/lvubuntu`

Vamos checar o tamanho da nova partição montada

`root@vm-testes:~# df -kh /dev/mapper/vg--teste-lvubuntu`
```
Sist. Arq.                      Tam. Usado Disp. Uso% Montado em
/dev/mapper/vg--teste-lvubuntu   45G   52M   42G   1% /dados/lvubuntu
```
### 11. Removendo um PV (Phisical Volume)

Para remover o disco do VG, utilize o comando abaixo
`root@vm-testes:~# vgreduce vg-teste /dev/sdb1`

Para remover um disco PV
`root@vm-testes:~# pvremove /dev/sdb1`
## Atualizando HP 1910 Switch Series

### 1 Download do firmware e instalação do OpenTFTPServer no MS Windows

#### 1.1 Faça o download do último firmware disponível no site da [hp](https://h10145.www1.hpe.com/Downloads/SoftwareReleases.aspx?ProductNumber=JE006A&lang=pt&cc=br&prodSeriesId=4218346);

#### 1.2 Defina o IP do MS Windows como static, no nosso caso iremos utilizar "192.168.1.1/24";

#### 1.3 Instale o OpenTFTPServer (disponível em "Instaladores");

#### 1.4 Copie a Imagem ".bin" recém baixada para "C:\OpenTFTPServer";

#### 1.5 Inicie o serviço executando o script "C:\OpenTFTPServer\RunStandAloneMT.bat";

### 2 Upgrade da versão do firmware

#### 2.1 Configure o IP da VLAN-interface 1 da switch como "192.168.1.2/24" e especifique o default gateway como "192.168.1.1";
`ipsetup ip-address 192.168.1.2 24 default-gateway 192.168.1.1`

#### 2.2 Faça o download do pacote ".bin" do servidor TFTP para o switch e atualize a imagem de software do sistema.
`upgrade 192.168.10.1 Switch1910.bin runtime`
```
File will be transferred in binary mode
Downloading file from remote TFTP server, please wait.../
TFTP: 10262144 bytes received in 71 second(s)
File downloaded successfully.
```

#### 2.3 Faça o download do pacote ".bin" do servidor TFTP para o switch e atualize a imagem de ROM de inicialização.
 `upgrade 192.168.10.1 Switch1910.bin bootrom`

Será informado que já existe um arquivo na memória flash. Confirme a sobrescrita!
```
The file flash:/Switch1910.bin exists. Overwrite it? [Y/N]:y
Verifying server file...
Deleting the old file, please wait...
File will be transferred in binary mode
Downloading file from remote TFTP server, please wait.../
TFTP: 10262144 bytes received in 61 second(s)
File downloaded successfully.
BootRom file updating finished!
```

#### Reinicie o switch agora para validar a imagem atualizada.
`reboot`

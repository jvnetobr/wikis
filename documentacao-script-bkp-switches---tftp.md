# Documentação script bkp_switches + TFTP

## Instalação TFTPD

Instale os pacotes abaixo

`root@arquivo:~# apt-get install tftpd-hpa`

Crie o arquivo de configuração deixando conforme abaixo

`root@arquivo:~# vim /etc/default/tftpd-hpa`

```
# /etc/default/tftpd-hpa 
TFTP_USERNAME="tftp"
TFTP_DIRECTORY="/dados/bkp_switches"
TFTP_ADDRESS="0.0.0.0:69"
TFTP_OPTIONS="-s -c -l
```

Crie o diretório "/dados/bkp_switches" para ser os arquivos transferidos pelo tftpd e defina as devidas permissões 

`root@arquivo:~# mkdir /dados/bkp_switches`

`root@arquivo:~# chmod -R 2775 /dados/bkp_switches`

`root@arquivo:~# chown -R tftp:tftp /dados/bkp_switches`

Reinicie o serviço tftpd-hpa

`root@arquivo:~# service tftpd-hpa restart`

ou

`root@arquivo:~# /etc/init.d/tftpd-hpa restart`

Agora nosso servidor ftpd está em execução, vamos testar criando um arquivo chamado "teste" dentro de "/dados/bkp_switches". Através de um outro servidor com o cliente tftp execute os passos conforme exibido abaixo.

`root@vm-testes:~# tftp 10.10.10.234`

`root@vm-testes:~# tftp> get teste`
```
Sent 0 bytes in 0.0 seconds
```
`root@vm-testes:~# tftp> quit`

`root@vm-testes:~# cat teste`

## Instalação "expect" e "mutt"

`root@arquivo:~# apt-get install expect expect-dev mutt`

## Execução script "bkp_switches.sh"

Vamos adicionar o script de [backup de switches](http://gitlab.tcm.ce.gov.br/Infraestrutura/Infraestrutura/blob/scripts-arquivo/bkp_switches.sh) no agendamento do cron para toda sexta-feira às 00:20 conforme abaixo.

`root@arquivo:~# crontab -e`

```
# Backup dos switches
0 20 * * 5 /root/scripts/bkp_switches.sh
```

O script "bkp_switches.sh" 

- Lista a data do ultimo backup realizado dos arquivos de configuração das switches, cria um diretório com nome backup.$DATA e move o ultimo backup para esse diretório;

- Com as variáveis declaradas (DATE, USERNAME, PASSWORD, HOST e TFTP), o script acessa cada switch (10.10.0.21 à 10.10.0.65) via telnet e envia uma copia do arquivo "startup.cfg" da swicth via tftp para o servidor de arquivos;

- Após finalização do backup dos arquivos de configuração, o script envia um e-mail com o ASSUNTO="BACKUP SWITCHES" e MENSAGEM="Backup dos Switches foi realizado com sucesso em $DATA_HORA." para os DESTINATARIOS="claudio.accr@gmail.com robertofreire42@gmail.com moaca2005@gmail.com moacir@tcm.ce.gov.br robertofreire@tcm.ce.gov.br";

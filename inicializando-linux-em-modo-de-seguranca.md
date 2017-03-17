## Entrar em "recovery mode"

Na tela do GRUB, escolha a opção "Opções avançadas para Ubuntu", após isso escolha a opção "recovery mode" do último kernel disponível conforme seguem as telas abaixo.

![grub01](/uploads/1a60de801ef63e59b4631494f491cb61/grub01.PNG)

![grub02](/uploads/30221ffeb7ebee596fd852ed628fc598/grub02.PNG)

## Remontar partições para escrita e leitura

Ao iniciar em "recovery mode", todas as partições serão montadas em modo apenas de leitura. Para remontar a partição em modo de escrita e leitura, faremos os procedimentos abaixo:

Vamos primeiramente identificar qual partição será remontada:

`root@vm-testes:~# df -h`
```
Sist. Arq.                           Tam. Usado Disp. Uso% Montado em
/dev/mapper/ubuntu--testvm--vg-root   24G  2,6G   20G  12% /
```

No exemplo vamos remontar a partição "/", agora bastar executar o seguinte comando:

`root@vm-testes:~# mount / -o remount,rw`

Agora faça as alterações no sistema de arquivos e não esqueça de reinicializar em modo multiusuário.
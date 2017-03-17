# Usando crontab para agendar tarefas nos servidores GNU/Linux

Podemos utilizar o comando "crontab -e"  para editar a tabela de agendamentos do cron do usuário ou editar o arquivo "/etc/crontab" com um editor de texto.

`root@arquivos-smb4:~# crontab -e`

ou

`root@arquivos-smb4:~# vim /etc/crontab`

#### Segue abaixo a sintaxe utilizada: 

[MINUTOS] [HORAS] [DIAS DO MÊS] [MÊS] [DIAS DA SEMANA] [USUÁRIO] [COMANDO] 

#### O valor de cada campo:

- MINUTOS: (0 a 59);
- HORAS: (0 a 23);
- DIAS DO MÊS: (1 a 31);
- MÊS: (1 a 12 -> Janeiro à Dezembro);
- DIAS DA SEMANA: (0 a 7 -> Domingo à Sábado, 0 e 7 equivalem ao Domingo);
- USUÁRIO: usuário que vai executar o comando (não precisa ser informado no uso do comando "crontab -e");
- COMANDO: comando que será executado

#### Observações:
- Podemos utilizar as 3 primeiras letras dos dias da semana em inglês (SUN,MON,TUE,WED,THU,FRI,SAT);
- Podemos utilizar o "asterisco" (*), especificando que o valor não será relevante;
- Podemos utilizar o "hífen" (-), especificando que será utilizado um intervalo entre os valores;
- Podemos utilizar a "vírgula" (,), especificando que será utilizada uma lista de valores;
- Podemos inserir comentários após o campo comando ou com o "cerquilha" (#) no início da linha;

#### Seguem abaixo alguns exemplos de agendamentos realizados pela crontab no servidor "arquivos-smb4" do TCM-CE:
```
# Limpeza dos diretórios Lixeira, Público e Digitalizacao
#  0   0  *   *   6     /bin/rm -rf /dados/lixeira/* >/dev/null 2>&1
 00   00  *   *   6     /bin/rm -rf /dados/Publico/* >/dev/null 2>&1
 00   00  *   *   6     /bin/rm -rf /dados/setores/Digitalizacao/* >/dev/null 2>&1
```
No exemplo acima, os comandos "/bin/rm -rf /dados/Publico/* >/dev/null 2>&1" e "/bin/rm -rf /dados/setores/Digitalizacao/* >/dev/null 2>&1" são executados às 00 horas e 00 minutos, toda sexta-feira, em todos os meses e em todos os dias do mês.  As duas primeiras linhas estão comentadas.
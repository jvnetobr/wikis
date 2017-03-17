# Administração / Manutenção - Aplicação GitLab

Com a finalidade de uma melhor administração da aplicação GitLab utlizada no TCM-CE, resolvemos listar abaixo alguns comandos úteis.

## Iniciar, parar e reiniciar serviços do GitLab
### Iniciar todos serviços
`sudo gitlab-ctl start`

### Parar todos serviços
`sudo gitlab-ctl stop`

### Reiniciar todos serviços
`sudo gitlab-ctl restart`

## Visualizar status dos serviços
`sudo gitlab-ctl status`
```
run: gitlab-workhorse: (pid 3148) 8192s; run: log: (pid 859) 8645s
run: logrotate: (pid 31432) 991s; run: log: (pid 861) 8645s
run: nginx: (pid 3159) 8191s; run: log: (pid 855) 8645s
run: redis: (pid 3161) 8191s; run: log: (pid 885) 8645s
run: sidekiq: (pid 4369) 7684s; run: log: (pid 857) 8645s
run: unicorn: (pid 4437) 7673s; run: log: (pid 852) 8645s
```
## Visualizar logs dos processos
### Visualizar todos logs
`sudo gitlab-ctl tail` 

### Visualizar logs específicos através do diretório "/var/log/gitlab"
`sudo gitlab-ctl tail gitlab-rails`

`sudo gitlab-ctl tail nginx/gitlab_error.log`



## Instalação e configuração inicial Sentry

Documentação sobre a instalação e configuração inicial da ferramenta de log de aplicações Sentry.

#### Instalação das ferramentas do python e das bibliotecas de dependências
`root@sentry:~# apt-get install python-setuptools python-pip python-dev libxslt1-dev gcc libxml2-dev libz-dev libffi-dev libssl-dev libpq-dev libyaml-dev libjpeg-dev`

#### Instalação do servidor PostgreSQL

`root@sentry:~# apt-get install postgresql`

#### Instalação do NGINX

`root@sentry:~# apt-get install nginx-full`

#### Instalação do Supervisor

`root@sentry:~# apt-get install supervisor`

#### Instalação do Redis

`root@sentry:~# apt-get install redis-server redis-tools`

#### Instalação da Virtualenv com o pip

`root@sentry:~# pip install -U virtualenv`

#### Criação do usuário "sentry"

`root@sentry:~# adduser sentry`

#### Criação do usuário e banco no PostgreSQL

`root@sentry:~# su - postgres`

`postgres@sentry:~$ psql template1`

`template1=# create user sentry with password 'senha-usuario';`

`template1=# create database sentrydb with owner sentry;`

`template1=# \q`

`postgres@sentry:~$ exit`

#### Entrar com o novo usuário "sentry" que foi criado

`root@sentry:~# su - sentry`

#### Criando o ambiente virtual 

`sentry@sentry:~$ virtualenv ~/sentry_app/`

#### Ativando o ambiente virtual

`sentry@sentry:~$ source ~/sentry_app/bin/activate`

## Instalação e configuração do Sentry

#### Instalação do Sentry

`sentry@sentry:~$ pip install -U sentry`

#### Agora vamos criar a configuração padrão do sentry, vamos utilizar o comando `init`com o argumento do diretório criado

`sentry@sentry:~$ sentry init`

#### Edite o arquivo `sentry.conf` com as informações de usuário e banco criados anteriormente

`sentry@sentry:~$ vim ~/.sentry/sentry.conf.py`
```
DATABASES = {
    'default': {
        'ENGINE': 'sentry.db.postgres',
        'NAME': 'sentrydb',
        'USER': 'sentry',
        'PASSWORD': 'tcm@enscnpstd',
        'HOST': 'localhost',
        'PORT': '5432',
        'AUTOCOMMIT': True,
        'ATOMIC_REQUESTS': False,
    }
}
```

#### Vamos adicionar no `config.yml` as informações referentes ao redis
`sentry@sentry:~$ vim ~/.sentry/config.yml`
```
redis.clusters:
  default:
    hosts:
      0:
        host: 127.0.0.1
        port: 6379
```

#### Execute o comando abaixo para serem  executados os scripts de migração do banco de dados. Será perguntado se você deseja criar o usuário como "superusuário", crie e faça logout do usuário "sentry"!

`sentry@sentry:~$ sentry upgrade`

`sentry@sentry:~$ exit`

#### Vamos criar o arquivo de configuração para a inicialização do "supervisord". Crie o arquivo e cole o conteúdo abaixo.
`root@sentry:~# vim /etc/supervisor/conf.d/sentry.conf`
```
[program:sentry-web]
directory=/home/sentry/sentry_app/
environment=SENTRY_CONF="/home/sentry/.sentry"
command=/home/sentry/sentry_app/bin/sentry start
autostart=true
autorestart=true
redirect_stderr=true
user=sentry
stdout_logfile=syslog
stderr_logfile=syslog

[program:sentry-worker]
directory=/home/sentry/sentry_app/
environment=SENTRY_CONF="/home/sentry/.sentry"
command=/home/sentry/sentry_app/bin/sentry celery worker
autostart=true
autorestart=true
redirect_stderr=true
user=sentry
stdout_logfile=syslog
stderr_logfile=syslog

[program:sentry-cron]
directory=/home/sentry/sentry_app/
environment=SENTRY_CONF="/home/sentry/.sentry"
command=/home/sentry/sentry_app/bin/sentry celery beat
autostart=true
autorestart=true
redirect_stderr=true
stdout_logfile=syslog
stderr_logfile=syslog
```
#### Execute os comandos abaixo para que as configurações do `supervisord` e `supervisorctl` sejam recarregadas.
`root@sentry:~# supervisord -c /etc/supervisor/supervisord.conf`

`root@sentry:~# supervisorctl -c /etc/supervisor/supervisord.conf`

`root@sentry:~# supervisorctl reread`

`root@sentry:~# supervisorctl update`

#### O status do serviço também pode ser verificado com o comando abaixo.
`root@sentry:~# supervisorctl status`
```
sentry-cron                      RUNNING   pid 18501, uptime 0 days, 2:51:59
sentry-web                       RUNNING   pid 18500, uptime 0 days, 2:51:59
sentry-worker                    RUNNING   pid 18502, uptime 0 days, 2:51:59
```

#### Configurando systemd

No Ubuntu 16.04 os arquivos são criados em `/etc/systemd/system/`. Será necessário criar 3 arquivos, um para cada serviço chamados `sentry-web.service`, `sentry-worker.service` e `sentry-cron.service` conforme abaixo.

`root@sentry:~# cd /etc/systemd/system/`

`root@sentry:~# vim sentry-web.service`
```
[Unit]
Description=Sentry Main Service
After=network.target
Requires=sentry-worker.service
Requires=sentry-cron.service

[Service]
Type=simple
User=sentry
Group=sentry
WorkingDirectory=/home/sentry/sentry_app
Environment=SENTRY_CONF=/home/sentry/.sentry
ExecStart=/home/sentry/sentry_app/bin/sentry run web

[Install]
WantedBy=multi-user.target
```

`root@sentry:~# vim sentry-worker.service`
```
[Unit]
Description=Sentry Background Worker
After=network.target

[Service]
Type=simple
User=sentry
Group=sentry
WorkingDirectory=/home/sentry/sentry_app
Environment=SENTRY_CONF=/home/sentry/.sentry
ExecStart=/home/sentry/sentry_app/bin/sentry run worker

[Install]
WantedBy=multi-user.target
```

`root@sentry:~# vim sentry-cron.service`
```
[Unit]
Description=Sentry Beat Service
After=network.target

[Service]
Type=simple
User=sentry
Group=sentry
WorkingDirectory=/home/sentry/sentry_app
Environment=SENTRY_CONF=/home/sentry/.sentry
ExecStart=/home/sentry/sentry_app/bin/sentry run cron

[Install]
WantedBy=multi-user.target
```

#### Para garantir que os serviços irão inicializar com o sistema, execute o comando abaixo.
`root@sentry:~# systemctl enable sentry-web.service`

Para finalizar a instalação do servidor acesse `http://10.10.10.115:9000` e preencha os campos da página.
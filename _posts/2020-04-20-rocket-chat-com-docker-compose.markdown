---
layout: post
title:  "Rocket Chat com Docker Compose"
date:   2020-04-20 20:49:03 -0300
categories: services
tags: docker
---

Rocket Chat no Docker.

- Para rodar esse docker compose deve ser feita a instalacao do NGNIX e configurado com um certificado digital, assim como manda a documentacao, para evitar confusao (como eu mesmo me confundi no inicio), tomei a liberdade de nao incluir a configuracao de um firewall e do FailtoBan como foi feito na documentacao original ( logico que para fins de teste apenas, em producao deve ser adotado o metodo da documentacao pois e mais seguro ).

## Etapas da configuracao:

1. Criar certificado digital SSL.
```
touch /etc/ngnix/certificate.key

touch /etc/ngnix/certificate.crt

sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/certificate.key -out /etc/nginx/certificate.crt
```
2. Instalar servidor ngnix.
```
sudo apt-get install nginx
```
2.1 Clonar o repositorio com docker-compose.yml OBS:(para dentro da pasta /var/www/ isso é importante).
```
cd /var/www/

git clone https://github.com/WesllenMatias/rocket.chat.git
```
3. Instalar o docker.
```
wget -qO- https://get.docker.com/ | sh
```
4. Instalar o docker-compose e dar permissao de execucao a ele.
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```
5. Configurar ngnix com o certificado.

5.1. Altere a permissao do arquivo do certificado.
```
sudo chmod 400 /etc/nginx/certificate.key

sudo openssl dhparam -out /etc/nginx/dhparams.pem 2048
```
5.2. Agora abra o arquivo de configuracao do NGNIX.( OBS? Altere o server_name e o proxy_pass com o DNS de sua preferencia )
```
sudo nano /etc/nginx/sites-available/default
```
5.3. Apague as configuracoes atuais e cole as abaixo.
```
# HTTPS Server
    server {
        listen 443 ssl;
        server_name rocket.chat;

        error_log /var/log/nginx/rocketchat_error.log;

        ssl_certificate /etc/nginx/certificate.crt;
        ssl_certificate_key /etc/nginx/certificate.key;
        ssl_dhparam /etc/nginx/dhparams.pem;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers 'ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA';
        ssl_prefer_server_ciphers on;
        ssl_session_cache shared:SSL:20m;
        ssl_session_timeout 180m;

        location / {
            proxy_pass http://rocket.chat:3000/;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            proxy_set_header Host $http_host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forward-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forward-Proto http;
            proxy_set_header X-Nginx-Proxy true;
            proxy_redirect off;
        }
    }
```
5.4. Reinicie o NGNIX.
```
sudo service nginx configtest && sudo service nginx restart
```
6. Crie as pastas onde vão ficar os volumes dos containers.
```
sudo mkdir -p /var/www/rocket.chat/data/runtime/db
sudo mkdir -p /var/www/rocket.chat/data/dump
```
7. Agora va ate a pasta /var/www/rocket.chat/ e execute o docker-compose.yml
```
docker-compose up -d

```

## Documentacao Original:
### https://rocket.chat/docs/installation/docker-containers/index.html

## Dicas:
Para um ambiente de teste agil, creio que seja mais produtivo usar o Vagrant e subir uma VM ubuntu com o docker ja instalado, estarei providenciando na sequencia o arquivo Vagrantfile para facilitar o ambiente de teste, afinal configurar VM step-by-step realmente torna o teste um martirio.

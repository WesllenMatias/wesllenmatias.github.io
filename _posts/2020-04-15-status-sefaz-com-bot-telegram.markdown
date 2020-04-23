---
layout: post
title:  "Status Sefaz com Bot Telegram"
date:   2020-04-15 11:43:03 -0300
categories: bot-telegram
tags: telegram
---

![nfe-icn](https://user-images.githubusercontent.com/46201054/56322970-d7553e80-6140-11e9-8c80-50ed7f5fc6ff.png)

Neste post vou mostrar como utilizar o bot telegram que desenvolvi para poder capturar o status atual da sefaz, para ter de forma rapida e na mão o status da Sefaz quando for necessário.


Para rodar o scrip como serviço em seu servidor siga os passos abaixo:

- Instale o jq e o lynx:

```shell
apt-get install jq -y

apt-get install lynx -y
```

- Clone o repositório:

```shell
git clone https://github.com/WesllenMatias/sefaz.git
```
- Abra o sefaz.sh e altere a variavel bot_token e adicione o token do seu bot telegram:

```shell
bot_token='[add aqui o token do seu bot]'
```
- Acesse a pasta sefaz e dê permissão de execução aos arquivos ShellBot.sh e sefaz.sh:

```shell
chmod +x sefaz.sh

chmod +x ShellBot.sh
```

- Para deixar o bot rodando como servico dentro de sua maquina execute os passos abaixo:

- Crie o arquivo sefaz.service dentro de /etc/systemd/system/

```c
[Unit]
Description=Sefaz

[Service]
Type=simple
RemainAfterExit=yes
ExecStart=/local/do/sefaz.sh start
ExecStop=/local/do/sefaz/sefaz.sh stop
ExecReload=/local/do/sefaz/sefaz.sh restart

[Install]
WantedBy=multi-user.target
```
- Execute os comandos abaixo:

```shellscript

systemctl daemon-reload
systemctl enable sefaz
```

_OBS: Dentro do Fonte **sefaz.sh** em source coloque o caminho absoluto de onde fica o ShellBot.sh_

## Resultado:
![Mensagem do Bot](/assets/img/tela-sefaz.png)
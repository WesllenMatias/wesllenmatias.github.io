---
layout: post
title:  "Status Sefaz com Bot Telegram"
date:   2020-04-15 11:43:03 -0300
categories: bot-telegram
tags: telegram
---

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
- Caso queira deixar ele rodando direto na máquina basta executar o comando abaixo:

```shell
    nohup sefaz.sh &&
```
**OBS: sefaz.sh e ShellBot.sh devem está no mesmo diretório.**
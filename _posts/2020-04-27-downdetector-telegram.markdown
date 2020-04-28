---
layout: post
title:  "Top 5 Downdetector via Telegram"
date:   2020-04-20 20:49:03 -0300
categories: services
tags: telegram
---

# BotTelegram Downdetector


Passo a Passo para usar o Bot:


## Instalando dependências

__São utilizadas algumas bibliotecas  que precisam ser instaladas:__

- Selenium
- pyTelegramBotAPI
- BeautifulSoup

__-OBS: O python utilizado é o Python3__

```shellscript

pip3 install selenium

pip3 install pyTelegramBotAPI

pip3 install bs4

```

## Clone o Repositório

```shellscript

git clone https://github.com/WesllenMatias/downdetector-telegram.git

```
## Crie o arquivo chamado config.py com o Token e o ID de quem vai receber a mensagem:

```Python

TOKEN = "Informe o Token do seu bot aqui"

```

## Execute o Bot e deixe ele rodando em sua instancia

```shellscript
python3 downdetector.py
```

**Pronto basta ir em seu bot no telegram e digitar /foradoar**


## Resultado:

![bot-downdetector](/assets/img/bot-downdetector.png)